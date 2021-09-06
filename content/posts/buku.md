+++
title = "buku <3"
description = "Started using buku recently to manage bookmarks and loving it so far"
date = 2021-08-21
paginate_by = 20
[taxonomies]
tags = ["cli","qs","using"]
+++

Ruined my plans to make my own link tracking systems in the best way possible.

Base [buku](https://github.com/jarun/buku) is great (like calling `buku --ai` and getting all your browser bookmarks!), but there are things I've added so far to make it even better, stored in [this repo](https://github.com/tadeaspaule/buku-utils)
tldr:
1. Keep track of time added
2. Rofi menu for opening/editing/deleting (made by [carnager](https://github.com/carnager/buku_run))
3. Newsboat integration

My boku db now powers the links shared on this site! I add a `share` tag when it's something I want displayed here, and use the `time_added` field to sort them.

[Zola](https://www.getzola.org/) + buku is a great combo thanks to tags. Not only can you see everything related to, for example, [cli](/tags/cli) (be it a link, a post, etc), but each tag has its own automatically generated [RSS feed](https://www.getzola.org/documentation/templates/feeds/). For example, a feed for `cli` is at `/tags/cli/feed.xml`.

