<h1>ExpNo 7 : Implement Alpha-beta pruning of Minimax Search Algorithm for a Simple TIC-TAC-TOE game</h1> 
<h3>Name:  MADHANN KUMAR V   </h3>
<h3>Register Number:212224040176      </h3>
<h3>DATE: 17.08.2026</h3>
<H3>Aim:</H3>
<p>
Implement Alpha-beta pruning of Minimax Search Algorithm for a Simple TIC-TAC-TOE game
</p>
<h1>GOALS of Alpha-Beta Pruning in MiniMax Search Algorithm</h1>

<h3>Improve the decision-making efficiency of the computer player by reducing the number of evaluated nodes in the game tree.</h3>
<h3>Tic-Tac-Toe game implementation incorporating the Alpha-Beta pruning and the Minimax algorithm with Python Code.</h3>
<h1>IMPLEMENTATION</h1>

The project involves developing a Tic-Tac-Toe game implementation incorporating the Alpha-Beta pruning with the Minimax algorithm. Using this algorithm, the computer player analyzes the game state, evaluates possible moves, and selects the optimal action based on the anticipated outcomes.

<h1>The Minimax algorithm</h1>

recursively evaluates all possible moves and their potential outcomes, creating a game tree.

<h1>Alpha-Beta pruning</h1>

Alpha–Beta (𝛼−𝛽) algorithm is actually an improved minimax using a heuristic. It stops evaluating a move when it makes sure that it’s worse than a previously examined move. Such moves need not to be evaluated further.

When added to a simple minimax algorithm, it gives the same output but cuts off certain branches that can’t possibly affect the final decision — dramatically improving the performance
<hr>
PROGRMA:
```
import time


class Game:
    def __init__(self):
        self.initialize_game()

    def initialize_game(self):
        self.current_state = [
            ['.', '.', '.'],
            ['.', '.', '.'],
            ['.', '.', '.']
        ]
        self.player_turn = 'X'

    def draw_board(self):
        for i in range(3):
            for j in range(3):
                print('{}|'.format(self.current_state[i][j]), end=" ")
            print()
        print()

    def is_valid(self, px, py):
        if px < 0 or px > 2 or py < 0 or py > 2:
            return False
        elif self.current_state[px][py] != '.':
            return False
        else:
            return True

    def is_end(self):
        # Check columns
        for i in range(3):
            if (self.current_state[0][i] != '.' and
                    self.current_state[0][i] == self.current_state[1][i] and
                    self.current_state[1][i] == self.current_state[2][i]):
                return self.current_state[0][i]

        # Check rows
        for i in range(3):
            if self.current_state[i] == ['X', 'X', 'X']:
                return 'X'
            elif self.current_state[i] == ['O', 'O', 'O']:
                return 'O'

        # Check main diagonal
        if (self.current_state[0][0] != '.' and
                self.current_state[0][0] == self.current_state[1][1] and
                self.current_state[0][0] == self.current_state[2][2]):
            return self.current_state[0][0]

        # Check other diagonal
        if (self.current_state[0][2] != '.' and
                self.current_state[0][2] == self.current_state[1][1] and
                self.current_state[0][2] == self.current_state[2][0]):
            return self.current_state[0][2]

        # Check for empty cells
        for i in range(3):
            for j in range(3):
                if self.current_state[i][j] == '.':
                    return None

        return '.'

    def max_alpha_beta(self, alpha, beta):
        maxv = -2
        px = None
        py = None

        result = self.is_end()

        if result == 'X':
            return (-1, 0, 0)
        elif result == 'O':
            return (1, 0, 0)
        elif result == '.':
            return (0, 0, 0)

        for i in range(3):
            for j in range(3):
                if self.current_state[i][j] == '.':
                    self.current_state[i][j] = 'O'

                    m, min_i, min_j = self.min_alpha_beta(alpha, beta)

                    if m > maxv:
                        maxv = m
                        px = i
                        py = j

                    self.current_state[i][j] = '.'

                    if maxv >= beta:
                        return (maxv, px, py)

                    if maxv > alpha:
                        alpha = maxv

        return (maxv, px, py)

    def min_alpha_beta(self, alpha, beta):
        minv = 2
        qx = None
        qy = None

        result = self.is_end()

        if result == 'X':
            return (-1, 0, 0)
        elif result == 'O':
            return (1, 0, 0)
        elif result == '.':
            return (0, 0, 0)

        for i in range(3):
            for j in range(3):
                if self.current_state[i][j] == '.':
                    self.current_state[i][j] = 'X'

                    m, max_i, max_j = self.max_alpha_beta(alpha, beta)

                    if m < minv:
                        minv = m
                        qx = i
                        qy = j

                    self.current_state[i][j] = '.'

                    if minv <= alpha:
                        return (minv, qx, qy)

                    if minv < beta:
                        beta = minv

        return (minv, qx, qy)

    def play_alpha_beta(self):
        while True:
            self.draw_board()

            self.result = self.is_end()

            if self.result is not None:
                if self.result == 'X':
                    print('The winner is X!')
                elif self.result == 'O':
                    print('The winner is O!')
                elif self.result == '.':
                    print("It's a tie!")

                return

            if self.player_turn == 'X':
                while True:
                    start = time.time()

                    m, qx, qy = self.min_alpha_beta(-2, 2)

                    end = time.time()

                    print(
                        'Evaluation time: {}s'.format(
                            round(end - start, 7)
                        )
                    )

                    print(
                        'Recommended move: X = {}, Y = {}'.format(
                            qx, qy
                        )
                    )

                    px = int(input('Insert the X coordinate: '))
                    py = int(input('Insert the Y coordinate: '))

                    if self.is_valid(px, py):
                        self.current_state[px][py] = 'X'
                        self.player_turn = 'O'
                        break
                    else:
                        print('The move is not valid! Try again.')

            else:
                m, px, py = self.max_alpha_beta(-2, 2)

                self.current_state[px][py] = 'O'
                self.player_turn = 'X'


    def main():
    g = Game()
    g.play_alpha_beta()


    if __name__ == "__main__":
    main()
  ```
```
OUTPUT:
```
PS C:\Users\B.Mohamed javid\Desktop\Fund of AI> python exp6.py
.| .| .| 
.| .| .| 
.| .| .| 

Evaluation time: 0.0475035s
Recommended move: X = 0, Y = 0
Insert the X coordinate: 0
Insert the Y coordinate: 0
X| .| .| 
.| .| .| 
.| .| .| 

X| .| .| 
.| O| .| 
.| .| .| 

Evaluation time: 0.0027564s
Recommended move: X = 0, Y = 1
Insert the X coordinate: 0
Insert the Y coordinate: 1
X| X| .| 
.| O| .| 
.| .| .| 

X| X| O| 
.| O| .| 
.| .| .| 

Evaluation time: 0.0001657s
Recommended move: X = 2, Y = 0
Insert the X coordinate: 2
Insert the Y coordinate: 0
X| X| O| 
.| O| .| 
X| .| .| 

X| X| O| 
O| O| .| 
X| .| .| 

Evaluation time: 3.41e-05s
Recommended move: X = 1, Y = 2
Insert the X coordinate: 1
Insert the Y coordinate: 2
X| X| O| 
O| O| X| 
X| .| .| 

X| X| O| 
O| O| X| 
X| O| .| 

Evaluation time: 9.5e-06s
Recommended move: X = 2, Y = 2
Insert the X coordinate: 2
Insert the Y coordinate: 0
The move is not valid! Try again.
Evaluation time: 2.26e-05s
Recommended move: X = 2, Y = 2
Insert the X coordinate: 2
Insert the Y coordinate: 2
X| X| O| 
O| O| X| 
X| O| X| 

It's a tie!
PS C:\Users\B.Mohamed javid\Desktop\Fund of AI> python exp7.py
.| .| .| 
.| .| .| 
.| .| .| 

Evaluation time: 1.1622944s
Recommended move: X = 0, Y = 0
Insert the X coordinate: 0
Insert the Y coordinate: 1
.| X| .| 
.| .| .| 
.| .| .| 

AI (O) is thinking...
AI evaluation time: 0.1170986s
AI chooses: X = 0, Y = 0
O| X| .| 
.| .| .| 
.| .| .| 

Evaluation time: 0.0152555s
Recommended move: X = 1, Y = 0
Insert the X coordinate: 1
Insert the Y coordinate: 1
O| X| .| 
.| X| .| 
.| .| .| 

AI (O) is thinking...
AI evaluation time: 0.0026317s
AI chooses: X = 2, Y = 1
O| X| .| 
.| X| .| 
.| O| .| 

Evaluation time: 0.0006893s
Recommended move: X = 1, Y = 0
Insert the X coordinate: 1
Insert the Y coordinate: 0
O| X| .| 
X| X| .| 
.| O| .| 

AI (O) is thinking...
AI evaluation time: 0.0002608s
AI chooses: X = 1, Y = 2
O| X| .| 
X| X| O| 
.| O| .| 

Evaluation time: 7.01e-05s
Recommended move: X = 0, Y = 2
Insert the X coordinate: 2
Insert the Y coordinate: 0
O| X| .| 
X| X| O| 
X| O| .| 

AI (O) is thinking...
AI evaluation time: 2.43e-05s
AI chooses: X = 0, Y = 2
O| X| O| 
X| X| O| 
X| O| .| 

Evaluation time: 8.8e-06s
Recommended move: X = 2, Y = 2
Insert the X coordinate: 0
Insert the Y coordinate: 2
The move is not valid! Try again.
Evaluation time: 2.41e-05s
Recommended move: X = 2, Y = 2
Insert the X coordinate: 2
Insert the Y coordinate: 2
O| X| O| 
X| X| O| 
X| O| X| 

It's a tie!
```
Result:
Thus,Implementation of Maximum Search Algorithm for a Simple TIC-TAC-TOE game wasa done successfully.
