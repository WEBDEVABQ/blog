---
layout: post
title: SharePoint 2013 Search - Default to Wildcard Search
comments: true
---

By default, SharePoint searches whole words or phrases. If a wildcard (*) is added to a search term, SharePoint will search partial words as well. However, this trick is not known by most end users, and only works if the target audience is technical or trained on this technique.

A simple fix is to edit the search results webpart and click Change Query. In the query text field, enter `{searchboxquery}*`, which will add the wildcard to the end of all search queries.

There are a couple of issues with this method:
1. Unlike a user-entered query with a wildcard, this method does not highlight the matched keyword in the search results.
2. Loading up the page without a search query will load all results (instead of no results found).
3. "Did you mean?" should be turned off, or every search term entered will trigger a "Did you mean?" with the same keyword entered by the user.
