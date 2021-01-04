# Halo-Ward

<table>
<?php
//Determine if a player as aligned three of its symbols and return the id of the player (1
//for X Player, -1 for O Player(Computer)). Otherwise return 0;
function isWinState($board){
    $winning_sequences = '012345678036147258048642';
    for($i=0;$i<=21;$i+=3){
        $player = $board[$winning_sequences[$i]];
        if($player == $board[$winning_sequences[$i+1]]){
            if($player == $board[$winning_sequences[$i+2]]){
                if($player!=0){
                    return $player;
                }
            } 
        }   
    }
    return 0;
}

//Player O plays its turn at random
function OsTurn($board){
    if(in_array(0,$board)){
        $i = mt_rand(0,8);
        while($board[$i]!=0){
            $i = mt_rand(0,8);
        }
        $board[$i]=-1;
    }
    return $board;
}

$winstate = 0;
$values = array();

if(empty($_GET['values'])){
    //initializing the board
    $values = array_fill(0,9,0);
    //determine who begins at random
    if(mt_rand(0,1)){
        $values = OsTurn($values);
    }
}else{
    //get the board
    $values = explode(',',$_GET['values']);
    //Check if a player X won, if not, player 0 plays its turn.
    $winstate = isWinState($values);
    if($winstate==0){
        $values = OsTurn($values);
    }
    //Check if a player 0 won
    $winstate = isWinState($values);    
}
//Display the board as a table
for($i=0;$i<9;$i++){
    //begin of a row
    if(fmod($i,3)==0){echo '<tr>';}
    echo '<td>';
    //representation of the player token, depending on the ID
    if($values[$i]==1){
        echo 'X';
    }else if($values[$i]==-1){
        echo 'O';
    }else{
        //If noone put a token on this, and if noone won, make a link to allow player X to
        //put its token here. Otherwise, empty space.
        if($winstate==0){
            $values_link = $values;
            $values_link[$i]=1;
            echo '<a href="tictactoe.php?values='.implode(',',$values_link).'">&nbsp;</a>';
        }else{
            echo '&nbsp;';
        }
    }
    echo '</td>';
    //end of a row
    if(fmod($i,3)==2){echo '</tr>';}
}
?>
</table>
<?php
//If someone won, display the message
if($winstate!=0){
    echo '<p><b>Player '.(($winstate==1)?'X':'O').' won!</b></p>';
}

PHP tic tac toe chintu white hat JR approved!!!!!!

