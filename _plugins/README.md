# Jekyll Plugin: RSS Feed Generator

Automatically generates and RSS 2.0 feed from your posts at the specified path.

## Usage

Place the rssgenerator.rb file in your jekyll _plugins directory and set the required configuration attributes in your _config.yml file. The attributes are:  

    name           - the name of the site
    url            - the url of the site
    description    - (optional) a description for the feed (if not specified will be generated from name)
    author         - (optional) the author of the site (if not specified will be left blank)
    copyright      - (optional) the copyright of the feed (if not specified will be left blank)
    rss_path       - (optional) the path to the feed (if not specified "/" will be used)
    rss_name       - (optional) the name of the rss file (if not specified "rss.xml" will be used)
    rss_post_limit - (optional) the number of posts in the feed

## License

This plugin is released under the MIT license.  
Feel free to use it and improve it.
