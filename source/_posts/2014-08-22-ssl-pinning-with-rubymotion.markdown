---
layout: post
title: "SSL pinning with Rubymotion"
date: 2014-08-22 14:50
comments: true
categories: Ruby RubyMotion iOS
---

To get started with what exactly is SSL pinning and why is this pretty important, please read this [blog post].

Just to summarize,

A client when making an SSL connection to the server does not check whether the certificate is exactly the one that was uploaded to the server. In browsers, the connections are made to n number of hosts and is obviously unknown prior to making a connection whereas in case of a mobile app, thats not the case. The host to which the connection is to be made is known to the app development team ( certainly in most cases ). Hence we could utilize the power of SSL pinning in mobile apps to add the extra security layer.

To implement SSL pinning, you need the certificate within the app.
I use the BubbleWrap library to make http connections in RubyMotion, but this could well be implemented with something like AFMotion as well. BubbleWrap internally uses the NSUrlConnection to make requests. We use the delegate method of NSUrlConnection ( willSendRequestForAuthenticationChallenge ) to implement the pinning.

I monkey patch ( familiar to Rubyists ) **BubbleWrap::HTTP::Query** to make the necessary changes. Be sure to check that the file type of the certificate is 'der' and its placed in the appropriate directory.

    class BubbleWrap::HTTP::Query
      def connection(connection, willSendRequestForAuthenticationChallenge: challenge)
        serverTrust = challenge.protectionSpace.serverTrust
        certificate = SecTrustGetCertificateAtIndex(serverTrust, 0)

        localCertData = (self.localCert)
        serverCertificateData = (SecCertificateCopyData(certificate))

        if(serverCertificateData.to_s == skabberCertData.to_s)
          credential = NSURLCredential.credentialForTrust(serverTrust)
          challenge.sender.useCredential(credential, forAuthenticationChallenge:challenge)
        else
          NSLog("The server's certificate does not match the servers. Cancelling the request.")
          challenge.sender.cancelAuthenticationChallenge(challenge)
        end
      end

      def localCert
        cerPath = NSBundle.mainBundle.pathForResource('something_domain_com', ofType:'der')
        (NSData.dataWithContentsOfFile(cerPath)).to_s
      end
    end



[blog post]:http://blog.lumberlabs.com/2012/04/why-app-developers-should-care-about.html  
