#Used color codes from this website: http://ozzmaker.com/add-colour-to-text-in-python/
#This has 2 player, difficulty levels, color, 3 size ships, play again function
play ="y" #makes the game repeat as long as you click y at the end
while play =='y':  
    import random
    print '\x1b\x5b1;31;36m\tHello\n'
    
    print 

    multi ="0" 
    mp=['1','2']
    while multi not in mp:
        multi = raw_input("single (1) or multiplayer (2)? ") #You input 1 or 2 to choose the number of players
    multi = int(multi)
    print   
    if multi == 2: #If you click 2, it will activate a place to put names
        player1 = raw_input('Player 1, pick a name: ')
        print
        player2 = raw_input('Player 2, pick a name: ')
        print

    difficulty ='0'
    dif=['1','2','3']

    while  difficulty not in dif: 
        difficulty = raw_input('Choose your difficulty level 1-3 :') 
        #You choose a number to change the size of the board (s)

    difficulty =int(difficulty)
    print

    def diff(difficulty):
        if difficulty == 1:
            return 5
        elif difficulty == 2:
            return 8
        elif difficulty == 3:
            return 30

    skill = diff(difficulty)

    board = []

    upper_list =[]
    for a in range(1,skill+1):
        upper_list.append(a)

    for x in range(0,skill):
      board.append(["~ "] * skill)
#Depending on the number clicked, it will append the board size and multiply the range.
    
    def print_board(board):
        nl=''
        for i in upper_list:
            if i<10:
                nl = nl+str(i)+'  '
            else:nl = nl+str(i)+' '
        print nl
        
        
        p=0
        for row in board:
            p=p+1
            print " ".join(row),p
            print

    print_board(board)
    
    def random_row(board):
      return random.randint(0,len(board)-1)

    def random_col(board):
      return random.randint(0,len(board[0])-1)

    ship_row = random_row(board)+1
    ship_col_1 = random_col(board)+1

    
    def second_part(board):

        if ship_col_1 == len(board):
            return ship_col_1 - 1
        if ship_col_1 == 1:
            return ship_col_1 + 1
        else:
            return ship_col_1 + 1

    ship_col_2 = second_part(board)

    def third_part(board):
        if (ship_col_2 == len(board) or ship_col_1 == len(board)) and \
           (ship_col_2 == len(board)-1 or ship_col_1 == len(board)-1):
            return len(board)-2
        elif ship_col_1 == 1:
                return ship_col_1 +2
        elif (ship_col_2 == 0 or ship_col_1 == 0) and \
             (ship_col_2 == len(board)-1 or ship_col_1 == len(board)-1):
            return ship_col_2 -2
        else: return  ship_col_2 +1   

    ship_col_3 = third_part(board)

    def fourth_r(board):
        if ship_row == 1:
            return 2
        if ship_row == len(board):
            return len(board)-1
        else:
            return ship_row-1

    def fourth_c(board):
        return max(ship_col_1,ship_col_2,ship_col_3) #Creates three parts

    ship_row_2 = fourth_r(board) 
    fourth_col = fourth_c(board)
    tries = 5 
    while tries > 0:

      if tries%2 == 0 and multi ==2:
          print "It's Your turn",player1
          print
      if tries%2 != 0 and multi==2:
          print "It's Your turn",player2
          print
      guess_row = raw_input("Guess Row:")
      guess_col = raw_input("Guess Col:")

      if guess_row.isdigit() == False or guess_col.isdigit() == False:    
          print 'Only integers'

      elif (int(guess_row) == ship_row and int(guess_col) == ship_col_1)or \
           (int(guess_row) == ship_row and int(guess_col) == ship_col_2)or \
           (int(guess_row) == ship_row and int(guess_col) == ship_col_3) or \
           (int(guess_row) == ship_row_2 and int(guess_col) == fourth_col):  
        if (tries%2==0 or tries%2!=0) and multi ==1:
            print "Congratulations, You sunk my battleship!!"
        elif tries%2==0:
            print "Congratulations "+player1+"! You sunk my battleship!!"

        elif tries%2!=0:
            print "Congratulations "+player2+"! You sunk my battleship!!"

        guess_row = int(guess_row)-1
        guess_col = int(guess_col)-1
        
        board[ship_row-1][ship_col_1-1] = "O "
        board[ship_row-1][ship_col_2-1] = "O "
        board[ship_row-1][ship_col_3-1] = "O "
        board[ship_row_2-1][fourth_col-1] = "O "
        board[guess_row][guess_col]='* '   
        print_board(board)
        break
      else:
        guess_row = int(guess_row)-1
        guess_col = int(guess_col)-1
        if guess_row < 0 or guess_row > len(board) or guess_col < 0 or guess_col > len(board)-1:
          print "Oops, that's not even in the ocean."
        elif(board[guess_row][guess_col] == "X"):
          print "You guessed that one already."          
        else:
          print "You missed my battleship!"
          board[guess_row][guess_col] = "\033[1;32mX\033[1;m"
          print_board(board)
          print "Number of tries you have left! "+ str(tries - 1) #Checks to see if sting subtracted all integers
          tries = tries - 1
          if  tries < 1:
              
              board[ship_row-1][ship_col_1-1] = "O "
              board[ship_row-1][ship_col_2-1] = "O "
              board[ship_row-1][ship_col_3-1] = "O "
              board[ship_row_2-1][fourth_col-1] = "O "
              print_board(board)
              print "Game Over!!"
              print
              break
    
    play = raw_input('repeat y or n: ')

print "Goodbye"
