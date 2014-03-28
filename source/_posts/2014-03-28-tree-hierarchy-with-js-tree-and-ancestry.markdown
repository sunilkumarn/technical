---
layout: post
title: "Tree hierarchy with Js-tree and ancestry"
date: 2014-03-28 21:24
comments: true
categories: Ruby Rails Javascript
---

Recently ended up reading one of my old codes where I had implemented a tree structure using the [Js-tree] jquery library. Hadn't tried out the [ancestry] gem then. Decided to create such a tree structure using ancestry. Though it ended up pretty fast and simple, it for sure doesnt look as pretty as the one made with the js-tree library. Hence, decided to try out a combo of both and using old school recursion it worked out pretty well.

To get it working, assume you have a categories models with the has_ancestry method.

    Category.arrange_serializable.to_json
    => [
            {
                "ancestry" => nil, "id" => 1, name: "OS",  "children" => [
                    { "ancestry" => "1", "id" => 2, name: "Linux" "children" => [] }
                ]
            }
        ]


Now since we use the js-tree library to display the hierarchy, we need to do a minor hack here. Js-tree requires the "data" key and some meta info in the json data to display the content of the node in the view. With a handy recursive method, we acheieve this.

In the *models/categories.rb*, we do this

    class << self
      def build_display_ancestry(category_hierarchy, tailored_hierarchy_data = [])
        category_hierarchy.each do |category_data|
          state = category_data['children'].empty? ? 'leaf' : 'open'
          custom_display_data = {
            :data => category_data['name'],
            :state => state,
            :metadata => { href: Rails.application.routes.url_helpers.category_path(category_data['id']) },
            :children =>  build_display_ancestry(category_data['children'], [])
          }
          tailored_hierarchy_data << custom_display_data
        end
        tailored_hierarchy_data
      end

      def viewed_ancestry
        build_display_ancestry(Category.arrange_serializable, [])
      end
    end

The *categories_controller* would have a 'view_ancestry' action for the js-tree method to fetch the data to be displayed,

    def view_ancestry
      render json: { data: 'Categories', state: 'open', metadata: { href: categories_path }, children: Category.viewed_ancestry }
    end

Now, some javascript for fetching the info from the server and displaying the data on view,

    $(document).ready(function(){
    $("#category-hierarchy").jstree({
        "json_data": {
            "ajax" : {
                "url" : $("#category-hierarchy").attr('data-path'),
                "data" : function (n) {
                    return { id : n.attr ? n.attr("id") : 0 };
                }
            }
        },
        plugins : ["themes", "json_data", "ui"],
        themes : {"theme": "default", "icons":false, "dots": true, "open":true},
        core: {"animation": 100}
    }).bind("select_node.jstree", function(e, data) {
            if (jQuery.data(data.rslt.obj[0], "href")) {
                window.location = jQuery.data(data.rslt.obj[0], "href");
            }
        })
    });

Yes, you would need a "#category-hierarchy" div to display the data.

Once all done and set, you would get a pretty neat tree structure without much effort. Something that looks like this,

<div>
 <img src = "{{ root_url }}/images/tree-hierarchy.png" alt = "Hierarchy">
</div>




[ancestry]:https://github.com/stefankroes/ancestry
[Js-tree]:https://github.com/tristanm/jstree-rails
