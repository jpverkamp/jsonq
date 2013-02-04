Usage:

    jsonq [options] [file ...]

Options:

    --[c]ount 
      print a count of each unique entry that matches the query, defaults to false
    --[f]ilter [not] query file
      only consider records where any field matching a json query is in the given file
      if not is present, invert the filter
      the default is no filter
    --[h]elp
      display usage
    --[o]ne [query]
      return only one entry from a given query (the first)
    --[q]uery [query]
      the query to run on the json objects, it should be a path within the json objects
      use '-' to return all values, this is the default
    --[s]ort [key|val]?
      defult is to not sort, use whatever default order
      if sorting, key is default; if key, sort by query; if val, sort by count

Input files can be either plaintext, gzipped, or - for stdin.

Some example commands using Twitter API responses:

Get a list of all user IDs in a series of files:

    jsonq.py --query user/id [tweet files] > [user id file]

Get a list of all IDs that are in the tweets, but not in user data:

    jsonq.py --query user/id [user data file] | jsonq.py --filter not user/id - --query user/id [tweet files] > [missing user id file]

Get a list of usernames associated with a list of IDs:

    jsonq.py --filter user/id [user id file] --query user/screen_name [tweet files]

Get the top n hashtags in a set of tweets:

    jsonq.py --count --query entities/hashtags/text [tweet files] | tail -n [n] | tac

Get the count of unique values from all Tweets using on IDs from a list (or not):

    jsonq.py --count --filter [not]? user/id [user id file] --query [query] [tweet files]

Get the registration date/time of a list of users from the originl data.

    jsonq.py --filter user/id [user id file] --query user/created_at --one user/id [tweet files]

Potentially interesting queries:

    user/default_profile
    user/default_profile_image
    user/friends_count
    user/followers_count
    user/screen_name
    entites/hashtags/text
    retweet_count
