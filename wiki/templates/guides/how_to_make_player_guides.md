# How to add your guide to the wiki

Adding your own strategy guides to the wiki can be a great way to share your knowledge of the game to develop strategies that everyone can use.

{table_of_contents}

## Writing good guides

Writing good guides can be hard and there's a lot of things that go into a good guide but there are a few key concepts that can help you write the best ones.

### Research

The most important thing to do is to make sure you've done your research and potentially tried a few different strategies. That doesn't mean your research has to be perfect, but the more research you do, the better the guide you write can be.

If you know how to read the game code, this is also a great way to better understand how things work. LLMs like Claude code can be quite helpful here as well to read through the codebase and find how mechanics work, although you should always do a sanity check to make sure it lines up with what you expect. Additionally, sometimes less is more and if a piece of code doesn't really have a useful place in your guide, you don't always need to include it, although you can always add it in a section at the end.

### Adding pictures and game data

People often tire of reading long blocks of text and adding visuals or actual game data references can be a great way to break up the flow.

[Custom generators](#custom-generators) allow you to incorporate game data into your guide however these are more complicated to add. If you're not comfortable working with these however, that's fine and having things like Excel tables, graphs, markdown tables, or anything else are great.

### Getting feedback

Before you publish to the wiki, it's a good idea to get some feedback on your guide. You can do this through the GitHub discussions or there's also a dedicated thread on the game's Discord which you can use to get feedback and refine your guide before adding it. If you'd like to join, you can do so at the link [here](https://discord.gg/vRxtXsBwQU)

## Adding guides to the wiki

Great, so you've figured out what you write and (hopefully) got some good feedback on the Discord or discussions which you've used to improve your guide. The next step is to add your guide to the wiki itself, starting off by creating a fork of the repo. If you're not familiar with the concept of forking repositories and creating pull requests, I'd recommend reading up about them on [GitHub docs](https://docs.github.com/en/pull-requests/get-started/pull-request-quickstart) or finding a how-to video which can help explain things.

Once you've created the fork, you have a couple options. If you want to create a typical guide which has images, and links to other pages, then you'll want to stick to the [standard approach](#getting-started). If you want to make your guide even better, consider incorporating real-time game data to your guide by following the [advanced approach](#advanced-custom-generators).

### Getting started

Generally, to add a page to the wiki, there's only a few things you need to do. The first thing is to add any images that you wish to reference into `wiki/images/guides`.

Then, you'll want to add a new markdown file under `wiki/templates/guides` with the actual content in your guide. There's no need to include the header with the title nor the related links at the bottom of some guides as these get automatically included in the standard template. You can see an example of this in `wiki/templates/guides/the_infinite_tower.md`.

Once that's done, you should replace any images with fields like `{{{{tower_bestiary}}}}`. When naming your field (like `tower_bestiary`), you should remove anything after the first dot `E.g. tower_bestiary.myfile.png → {{{{tower_bestiary}}}}`. Additionally, for any links to other pages, you can link to them directly using normal markdown links but ideally, you should link to them using a field with their page_id which is typically defined in [`pages.py`](https://github.com/tristinbaker/IdleFantasy/blob/da2c82a048bc6f0e416d9514421eadaf958a407c/wiki/src/pages.py#L64-L126).

Finally, once you've added any visuals and replaced the images with fields, all you have to do is add your guide to `wiki/templates/guides/guides.yml`. When defining it, you need to specify the id (that's the first level) and some parameters as described below:

| Parameter        | Description                                                                                                                                                                                                                                                    | Required?                   |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------|
| title            | This is the title of your guide, and it shows up at the top of the page.                                                                                                                                                                                       | Yes                         |
| author           | This is you! If multiple people made the guide, feel free to put some other name or even multiple names here.                                                                                                                                                  | Yes                         |
| last_updated     | This indicates when the guide was last updated and helps players know when the guide is most relevant, particularly as mechanics do change sometimes. The preferred format for this parameter is YYYY-MM-DD.                                                   | Yes                         |
| images           | This is a list of images that you use in your guide. Any image which you convert to a field must be listed here with the full filename.                                                                                                                        | No                          |
| page_links       | If you want to refer to other pages, this is the best way to link to them and is a list of page IDs. If you want to link to guides, these should be the id for the guide with the prefix of guide_. E.g. For the_infinite_tower, use guide_the_infinite_tower. | No                          |
| related_pages    | These page links are what gets listed at the bottom of a guide in the 'Related pages' section. It's highly recommended to add any page IDs of other related pages here as it can help players find related content, particularly for in-game pages.            | No                          |
| custom_generator | This is used when creating pages which incorporate game data. If you want to use custom generators, see [advanced setup](#advanced-custom-generators).                                                                                                         | Invalid for standard guides |

> If you want to add a table of contents to your guide, simply add {{{{table_of_contents}}}} at the top of your page and it'll be included

### Advanced: Custom generators

Custom generators are a great way to improve your guide by incorporating actual game data or more advanced formats. To get started, it's highly recommended to have a look at {getting_started_wiki_link}. While creating custom generators for guides isn't the same, they share a lot of traits with creating standard wiki pages and learning those can give you a lot of background.

To use custom generators, you first need to follow the steps for [Getting started](#getting-started) and create a guide using the standard method. The only difference is that for custom-generated pages, you can't have the `images` or `page_links` parameters and you'll need to set the `custom_generator` parameter to something unique.

Once you've done this, you can create a custom generator function in the section dedicated to custom player guides in `pages.py`. For it to work correctly, you'll need to define a function which takes in the guide you created as a parameter and then returns the properly formatted guide. Finally, simply add a reference to your generator in PLAYER_GUIDE_GENERATORS and that's all there is to it. If you try running the wiki, you should be able to see it show up in the sidebar in all its glory.

#### Using helper functions

When using custom generators, it's important to use helper functions wherever you can. These help ensure consistency across the wiki and minimise any code bloat. You can find a number of helper functions in the helper section of `pages.py` which can help you better format your pages.

Additionally for guides, whenever you wish to reference images, you should still keep them in `wiki/images/guides` but then reference them using `html_image` and ensure they have the class `guide-img`. 