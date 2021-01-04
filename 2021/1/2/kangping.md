# State machine Note
## Advantage of state machine
1. tidy interface
2. requires fixed/limited resources
3. holds no opinions about input/output (quite the opposite to functional programming?)
4. compact/concise implementation
5. easy to reason

## Examples
### Morse code decoder
Morse code decoder is a function that maps a sequence of `-` and `.` to characters (we use `0` to indicate end of the code sequnces.) We can obviously build the full mapping into a huge switch statement. However, here the author shows that We can view the return value of the function as a state, and each `-` or `.` within the sequence changes the state of the return value. The function rule can then be built into a trie (which is backed by an array). Very elegant/efficient indeed. 

### UTF-8 decoder
Here the logic structure is no longer a "tree", i.e. node would have multiple parents, however it is still a Direct Acyclic Graph. In theory we could still back the full states transition graph using an array. However, for some nodes the number of childern is relatively big, therefore, using a table to back the graph seems more reasonable. 

### Word count
This one seems the most natural of all. The easiest way would be to represent the state as a two element tuple (count, type). Non-whitespace change tuple (count, A) to (count, B), change tuple (count, B) to (count+1, B). whitespace keeps tuple (count, A) in (count, A), changes tuple (count, B) to (count, A). Here, the author uses the sign of int to represent the second element of the tuple, created some extra saving.

### Coroutines and generators

## Generate comments
To maintain a "state" is typically memory efficient due to the organization of the modern computer, i.e. It fits with how computer works. However, the obvious drawback is that maintaining a ever-switching state is not typically how computation works (computation is typically thought of as functions, which is very strict about input/output.) Switching from a functional approach to a state machine approach means that: 1. You need to construct the mapping in a clever way. 2. You need to write comprehensive tests, ideally cover all acceptable inputs. 