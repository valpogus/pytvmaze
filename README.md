## **Notice: pytvmaze 1.x is being deprecated and 2.x is being developed.**
**If you make use of the `get_show()` function you will need to modify your code for 2.x.  There will
be a new class `TVMaze()` which `get_show()` will be a method of.  This new class will also enable
the [new premium features that TVMaze is rolling out](http://www.tvmaze.com/blogs/14/announcing-tvmaze-premium)
by allowing you to create an authenticated `TVMaze()` instance with your username and api key to
access premium features.  Please see the new 2.x branch for more info.  Note that the premium
features are not yet available so any code in 2.x does not yet work and has not been tested, and
therefore is subject to change once we see how the new features work.  Once 2.x is stable I will
create a new 1.x branch and merge 2.x into master.**

To install:

```$ pip install pytvmaze```

**Basic usage**

    >>> import pytvmaze

    # Return list of Show objects from the TVMaze "Show Search" endpoint
    >>> shows = pytvmaze.get_show_list('stargate')
    >>> for show in shows:
    ...     print(show)
    Stargate Atlantis
    Stargate Universe
    Stargate: Infinity
    Stargate SG-1

    # Get the best match as a show object using the name of a show
    >>> show = pytvmaze.get_show(show_name='dexter')
    >>> print(show)
    Dexter
    >>> print(show.name, show.status, show.maze_id)
    Dexter Ended 161

    # Get a show object using a shows tvmaze id
    >>> show = pytvmaze.get_show(maze_id=161)
    >>> print(show)
    Dexter

    # Get a show object using a shows tvdb, tvrage id, or IMDB id
    >>> show = pytvmaze.get_show(tvdb_id=79349)
    >>> show = pytvmaze.get_show(tvrage_id=7926)
    >>> show = pytvmaze.get_show(imdb_id='tt3107288')

    # Iterate over all episodes (full episode list available at Show() level)
    >>> show = pytvmaze.get_show(maze_id=161, embed='episodes')
    >>> for episode in show.episodes:
    ...     print(episode.title)
    Dexter
    Crocodile
    Popping Cherry
    etc...

    # You can also use:
    >>> for season in show:
    ...     for episode in season:
    ...         ...

    # Iterate over specific season (season 2 for example)
    >>> for episode in show[2]:
    ...     print(episode.title)
    It's Alive!
    Waiting to Exhale
    An Inconvenient Lie
    etc...

    # Get a specific episode with: show[season][episode]
    >>> ep = show[1][8]
    >>> print(ep)
    S01E08 Shrink Wrap
    >>> print(ep.title)
    Shrink Wrap

    # Embed cast in Show object
    >>> show = pytvmaze.get_show(maze_id=161, embed='cast')
    >>> show.cast.people
    [<Person(name=Michael C. Hall,maze_id=29740)>,
    <Person(name=Jennifer Carpenter,maze_id=20504)>,
    etc.]

    >>> show.cast.characters
    [<Character(name=Dexter Morgan,maze_id=41784)>,
    <Character(name=Debra Morgan,maze_id=41786)>,
    etc.]

    # Show updates
    >>> updates = pytvmaze.show_updates()
    >>> updates[1]
    <Update(maze_id=1,time=1444852010)>
    # Time format is seconds since epoch - timestamp attribute gives datetime object
    >>> print(updates[1].timestamp)
    2015-10-14 12:46:50

**Search with qualifiers**

You can add the following qualifiers to your search:
```
show_year
show_network
show_language
show_country
show_web_channel
```
These qualifiers will be matched against the following show attributes: premier year, country, network name, and language.

    >>> show = pytvmaze.get_show(show_name='utopia', show_year='2014', show_country='au', show_network='abc')
    >>> show.premiered
    2014-08-13
    >>> show.network['name']
    ABC

**Show() Season() and Episode() class attributes**

There are many possible attributes of the Show class, but since TV Maze is full of user contributions and always being updated, shows will have different available attributes.  Possible attributes are:

    ## Show object attributes ##
    show.status
    show.rating
    show.genres
    show.weight
    show.updated
    show.name
    show.language
    show.schedule
    show.url
    show.image
    show.externals # dict of tvdb and tvrage id's if available
    show.premiered
    show.summary
    show.links # dict of previousepisode and nextepisode keys for their links
    show.web_channel
    show.runtime
    show.type
    show.id
    show.maze_id # same as show.id
    show.network # dict of network properties
    show.episodes # list of Episode objects
    show.seasons # dict of Season objects
    show.cast
    show.next_episode
    show.previous_episode

    ## Season object attributes ##
    season.show # parent show object, only present when Season object was created via embedding
    season.episodes # dict of episodes
    season.id
    season.url
    season.season_number
    season.name
    season.episode_order
    season.premier_date
    season.end_date
    season.network
    season.web_channel
    season.image
    season.summary
    season.links

    ## Episode object attributes ##
    episode.title
    episode.airdate
    episode.url
    episode.season_number
    episode.episode_number
    episode.image
    episode.airstamp
    episode.runtime
    episode.maze_id
    episode.summary
