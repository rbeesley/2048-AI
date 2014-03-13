# 2048 AI

AI for the game [2048](https://github.com/gabrielecirulli/2048).

See it in action [here](http://ov3y.github.io/2048-AI/). (Hit the auto-run button to let the AI attempt to solve it by itself)

The algorithm is iterative deepening depth first alpha-beta search. The evaluation function tries to keep the rows and columns monotonic (either all decreasing or increasing) while minimizing the number of tiles on the grid.

You can tweak the thinking time via global var `animationDelay`. Higher = more time/deeper search.

~~I think there are still some bugs as it tends to make some weird moves and die during the endgame, but in my testing it almost always gets 1024 and usually gets very close to 2048, achieving scores of roughly 8-10k.~~

The better heuristics now give it a success rate of about 90% in my testing (on a reasonably fast computer).

### Suggested Improvements

1.  Caching. It's not really taking advantage of the iterative deepening yet, as it doesn't remember the move orderings from previous iterations. Consequently, there aren't very many alpha-beta cutoffs. With caching, I think the tree could get pruned much more. This would also allow a higher branching factor for computer moves, which would help a lot because I think the few losses are due to unexpected random computer moves that had been pruned.

2. Put the search in a webworker. Parallelizing minimax is really hard, but just running it like normal in another thread would let the animations run more smoothly.

3. ~~Evaluation tweaks. There are currently four heuristics. Change the weights between them, run a lot of test games and track statistics to find an optimal eval function.~~

4. Comments and cleanup. It's pretty hacky right now but I've spent too much time already. There are probably lots of low-hanging fruit optimizations.

### Further exploration

1. Controls to set upper limit in powers of two. 2 - 4096,8192,16384,32768, etc.
2. Ability to change grid size.
3. Slider to control search depth.
4. Store the result of multiple runs.
5. Plot the distribution density of how many times the final tile appears in each type of location, accounting for symetery. 1:1=1(a) 2:2=1(a) 3:3=3(a,b/b2) 4:4=3(a,b/b2) 5:5=6(a,b,c/b2,bc/c2) 6:6=6(a,b,c/b2,bc/c2)
7:7=10(a,b,c,d/b2,bc,bd/c2,cd/d2) 8:8=10(a,b,c,d/b2,bc,bd/c2,cd/d2) 9:9=15(a,b,c,d,e/b2,bc,bd,be/c2,cd,ce/d2,de/e2) 10:10=15(a,b,c,d,e/b2,bc,bd,be/c2,cd,ce/d2,de/e2) https://oeis.org/A008805
6. When an auto-run wins or looses, it should automatically restart after a grace period.

### Questions to answer
1. Is there a correlation for winning, of the upper limit to the given grid size? Is there a maximum power of 2 tile per grid size? How to prove? 
2. Using the default solver, how does the percentage chance of winning vary as a function of limit and grid size?
