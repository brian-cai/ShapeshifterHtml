# ShapeshifterHtml
Using A* to solve Neopet's Shapeshifter Game.

Still looking for better heuristics, and optimizing speed. (It gets pretty slow when the board is large enough)

## How to Play
The goal is to use all the rotation pieces to make the entire board the goal picture.

<img src="https://i.imgur.com/uqD0tvT.png" height="50%" width="50%">

Example of the hardest level:
https://www.youtube.com/watch?v=M0fklfvPfAQ

Playing it yourself:
http://www.neopets.com/games/game.phtml?game_id=151

You can use my account:

username: shapeshiftersolver

password: algorithm123

## How to Use

### Command Line
Open Terminal, or whatever you have python on and clone this repository, and go into it:
```
git clone github.com/brian-cai/ShapeshifterHtml.git 
cd ShapeshifterHtml
```
Then, run `python shapeshifter.py` or `python3 shapeshifter.py` or whatever floats your boat.

You may run into issues with `from bs4 import BeautifulSoup`. This can be solved with getting pip and installing the appropriate modules with commands such as `pip3 install beautifulsoup4`. If you're using something like python IDLE, or using windows, there are other ways of installing bs4 such as `python -m pip install BeautifulSoup4` on cmd.

### Using your own levels
While collecting html puzzles, it occurred to me that the puzzle actually dynamically changes (the levels are not necessarily identical each time you play). To replace the puzzle, just inspect the element and select 'edit as html', and save the contents as `something.html`

Example: 

<img src="https://i.imgur.com/hLT7Mgf.png" height="50%" width="50%">


Inside shapeshifter.py, you just change the html file listed in

```gamemap, pieces, cycle, goalpiece = shapeshifter_html.get_shapeshifter_config('<name_of_file.html>')```

For instance, to use the level 4 file which is in the htmllevels folder:

```gamemap, pieces, cycle, goalpiece = shapeshifter_html.get_shapeshifter_config('htmllevels/level4.html')```

If you are curious what the gameboard, pieces, cycle, or goal is, you can print it out before hand by inserting

```shapeshifter_html.print_shapeshifter_html('<name_of_file.html>')```

It's already written for you to uncomment out.

You can also open the html file yourself in chrome or other browsers to see what the board looks like.

## Understanding the code

### HTML Parser
We are fetching the board through html parsing via `shapeshifter_html.py` and feeding the returned variables into a `ShapeShifterSearchProblem` object.

The object solves the problem using the A* search algorithm in `search.py` and keeps track of game states using a Counter from `util.py`.

### Search Heuristics

We are using Berkeley AI's skeleton (CS188) for A* search in `search.py` and their Counter for the dictionary in `util.py`.

We originally received the skeleton code from Georgia Tech's CS3600 class and implemented it ourselves. 

For all our heuristics we tested it on the "hard coded" level in `shapeshifter.py`

```
    # uncomment for hardcoded level
    '''
    pieces = (
        ((3, 3), ((0, 1, 1, 0), (0, 1, 1, 0), (1, 1, 0, 0), (0, 0, 0, 0))),
        ((2, 2), ((1, 1, 0, 0), (1, 1, 0, 0), (0, 0, 0, 0), (0, 0, 0, 0))),
        ((3, 3), ((1, 0, 1, 0), (1, 0, 1, 0), (1, 1, 1, 0), (0, 0, 0, 0))),
        ((3, 3), ((1, 0, 0, 0), (1, 1, 1, 0), (0, 0, 1, 0), (0, 0, 0, 0))),
        ((2, 2), ((1, 0, 0, 0), (1, 1, 0, 0), (0, 0, 0, 0), (0, 0, 0, 0))),
        ((2, 3), ((1, 1, 0, 0), (0, 1, 0, 0), (1, 1, 0, 0), (0, 0, 0, 0))),
        ((3, 3), ((1, 1, 0, 0), (0, 1, 1, 0), (1, 1, 0, 0), (0, 0, 0, 0))),
        ((3, 2), ((0, 1, 0, 0), (1, 1, 1, 0), (0, 0, 0, 0), (0, 0, 0, 0))),
        ((3, 3), ((1, 0, 1, 0), (1, 1, 1, 0), (1, 0, 1, 0), (0, 0, 0, 0))),
        ((2, 1), ((1, 1, 0, 0), (0, 0, 0, 0), (0, 0, 0, 0), (0, 0, 0, 0))),
        ((3, 3), ((0, 1, 0, 0), (1, 1, 1, 0), (0, 1, 0, 0), (0, 0, 0, 0)))
    )
    gamemap = ((2, 1, 0, 0),(1, 0, 1, 0),(2, 2, 0, 0),(0, 0, 0, 0))
    cycle = [0,1,2]
    goalpiece = 0
    '''
```

#### Heuristic 1: Blind equidistance from goal state

```
return sum(sum([bool(y != problem.goal_rank) for y in x]) for x in gamemap)
```

Nodes Expanded on Test Case: 47859

#### Heuristic 2: Blind equidistance distance from goal state if there are enough pieces remaining to rotate four corners, otherwise weighted sum of distances from goal state

```
    if (len(piecesleft) > 7):
        for x in gamemap:
            for y in x:
                htotal = (htotal + y) #the more rotations the further away
    else:
        for x in gamemap:
            for y in x:
                htotal = htotal + bool(y != problem.goal_rank) #the more rotations, the less of a difference
```

Nodes Expanded on Test Case: 13691

## What's Next
Thinking of new and better heuristics 

Implementing caching for heuristics

Making comparisons smoother by not requiring iteration through the entire gamestate

Implement a UI that allows an uploaded html + host it on the cloud

## Built With
* [BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/) - HTML Parser for Python
* [CS188 Skeleton](http://ai.berkeley.edu/search.html) - Berkeley AI Pacman homework assignment skeleton

## Authors

* **Andrew Young** - [Github](https://github.com/catatonicTrepidation/)
* **Brian Cai** - [Github](https://github.com/brian-cai)
