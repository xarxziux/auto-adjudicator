# Auto-adjudicator Python Script #

The auto-adjudicator Python script is a single-purpose tool that was borne from my more general Python Analysis Tool and shares much the same code, but is unlikely to be as broadly relevant.  This tool is designed to work in conjunction with [CuteChess](https://github.com/cutechess/cutechess) in order to build up a higher-quality opening book for ICCF use than can be obtained through downloading database of master games.

One problem with using a master game database to build an opening library, is that there is a unjustified assumption that the final result of the game is the logical conclusion of the player's opening choice, which is not true at all.  The auto-adjudicator, in conjunction with CuteChess, tries to solve that problem with the following sequence:

1. Select the position you want to examine.

2. Select the engines you want to use to play this position.  Stronger engines will presumably produce higher-quality games, but "weaker" (if an engine rated 3000+ ELO can be called weak) engines can sometimes find ideas that Stockfish, Komodo, etc. overlook.  I'm still trying to find the right balance between using only the top engines for the most reliable results, and letting Critter, BlackMamba, Equinox, etc. have their say as well.

3. Use CuteChess to run an engine tournament between these engines at slow time controls.  I usually bias the tournaments a little so that the top engines get more games.

4. Set CuteChess's options so that the game will be adjudicated as a draw after so many moves (for example 15-20 after the given position), unless a high threshold is reached.  This was, you are only looking at the situation a little after the opening moves.  I don't see any benefit in playing the whole game out and you are examining the opening position, not the whole game.

5. Run the resulting PGN though the auto-adjudicator script using your strongest engine at a deep search depth (for me, that's Stockfish 7 at a depth of 30 ply) using modest thresholds.  The setting I use are 70 cp for White and 50 cp for Black.  That means that if Stockfish assess the final position as 70 cp in White's favour or 50cp in Black's favour, then the result is considered won by that engine.  This does not mean that the game is won, and is probably pretty meaningless for OTB chess between unassisted humans, but for computer-assisted chess on ICCF where every cenitpawn counts, a 50cp advantage for Black after just 30 or so moves would be a highly-desireable result.

Now you can use the results from this sequence, along with raw analysis from the top engines, to get some idea of what is going on in a particular position, or perhaps find paths that may be more promising.  From my own use of the script, I've come across some very interesting results.  For example, sometimes one of the top engines just doesn't get a particular position at all, and loses badly even to the weaker engines.  I also now know, for example, that even the top engines don't understand the King's Indian Defence at all, and will happily assess a totally lost position as winning only to change their mind just 4-5 moves later.

# Set-up #

This script is written in Python 3.5 and has not been tested for backwards compatilibity.  It is also light on set up requirements and has only one dependency, [python-chess](https://github.com/niklasf/python-chess).  The only other requirements are a trusted engine (the hard-coded default is 64-bit Stockfish 7 for Linux, to change this you need to modify the source) and, of course, a PGN file of games to be adjudicated (this defaults to ptestsin.pgn, again you need to modify the source to change this).

# Development State #

In its current state, the script is not very adaptible or user-friendly as the only way to set the default options are to modify the source.  It has also only been tested under a very limited number of systems (specifically two Ubuntu Linux machines and a slow Win 10 laptop) so may not work on other systems.  However, it is functional and serves my own purposes so while is it nearly daily use, it is not currently in active development.  It may receive updates as I add features to [my other chess project] (https://github.com/xarxziux/xpat), but that project has been temporarily shelved as I have other priorities to take care of first.

