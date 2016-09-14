### Room settings:
Width: 300
Height: 600
Background: #000000

### Object Create Event:
```
board = ds_grid_create(10,20); //generally you'd want to clear a grid. But I guess this is fine.
placeable = ds_grid_create(4,4);
```
### Object Draw Event:
```
room_speed = 30+score + keyboard_check(vk_down)*20;
for (yy=0;yy<20;yy++) { //go through all rows drawing and clearing
    for (xx=0;xx<10;xx++) {
        rowCleared = (board[# xx,yy] != false && (xx==0 || rowCleared != false)); //check if the current row can be cleared. We never need to initalize this varible since it will always return true if xx == 0
        draw_rectangle_colour(30*xx,30*yy,30*xx+28,30*yy+28,board[# xx,yy],board[# xx,yy],board[# xx,yy],board[# xx,yy],0); //draw the board
        if (yy == 0 && board[# xx,yy] != false && solid == false) { //if your stuff is on the top row you lose
            show_message("GAME OVER!#Score:" + string(score) + string(solid++)) //use a built in varible to display the 0. Incriment the varible to not trigger this box twice
            game_restart(); }   }   
    if (rowCleared == true) {
        for (var i=((yy)*10-1);i>=0;i--) //if the row is clear move down everything
            {board[# i mod 10,(i div 10)+1] = board[# i mod 10,i div 10]}
        window_set_caption(string(++score) + "0")   }   } //Add the 0 so we can hide that the other 0 up top is a varible that's incremented
if (ds_grid_get_max(placeable,0,0,3,3) <= 0) { //choose a piece and place it on top, the pieces are ds grids saved
    ds_grid_read(placeable,choose("590200000300000003000000000000000000000000000000000000000000000014006E41000000000000000000000000000000000000000000000000000000000000000014006E41000000000000000014006E41000000000000000000000000000000000000000014006E41000000000000000000000000", "590200000300000003000000000000000000000000000000000000000000000000006E40000000000000000000000000000000000000000000000000000000000000000000006E40000000000000000000006E40000000000000000000000000000000000000000000000000000000000000000000006E40", "59020000030000000300000000000000000000000000000000000000000000000000000000000000000000000000EE4000000000000000000000000000000000000000000000EE4000000000000000000000EE4000000000000000000000000000000000000000000000EE40000000000000000000000000", "5902000003000000030000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001EE4400000000000000000001EE4400000000000000000001EE4400000000000000000000000000000000000000000000000000000000000000000001EE440", "590200000300000003000000000000000000000000000000000000000000000000000000000000000000000000006E41000000000000000000006E41000000000000000000006E41000000000000000000006E41000000000000000000000000000000000000000000000000000000000000000000000000", "5902000004000000040000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001E6E410000000000000000001E6E410000000000000000001E6E410000000000000000001E6E41000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000", "5902000002000000020000000000000000000000001EEE400000000000000000001EEE400000000000000000001EEE400000000000000000001EEE40"));
    place = "4"+string(-ds_grid_width(placeable)); }
else {
    var canMove = "111"  //initiliaze varibles. Save what direction you can go as a string so we can add it all on one line
    for (var i = 0; i<ds_grid_width(placeable)*ds_grid_width(placeable); i++) { //go through the placeables drawing and calculating
            if (placeable[# i div ds_grid_width(placeable),i mod ds_grid_width(placeable)] != false) { //look for collisions only if that frame has a block in that spot
                draw_rectangle_colour(30*((i div ds_grid_width(placeable))+(real(string_char_at(place,1))-1)),30*((i mod ds_grid_width(placeable))+real(string_delete(place,1,1))),30*((i div ds_grid_width(placeable))+(real(string_char_at(place,1))-1))+28,30*((i mod ds_grid_width(placeable))+real(string_delete(place,1,1)))+28,placeable[# i div ds_grid_width(placeable),i mod ds_grid_width(placeable)],placeable[# i div ds_grid_width(placeable),i mod ds_grid_width(placeable)],placeable[# i div ds_grid_width(placeable),i mod ds_grid_width(placeable)],placeable[# i div ds_grid_width(placeable),i mod ds_grid_width(placeable)],0);
                canMove  = string((!((i mod ds_grid_width(placeable))+real(string_delete(place,1,1))>18 || board[# (i div ds_grid_width(placeable))+(real(string_char_at(place,1))-1),(i mod ds_grid_width(placeable))+real(string_delete(place,1,1))+1] >= true) && string_char_at(canMove,1)  == "1"))  +  string((!((i div ds_grid_width(placeable))+(real(string_char_at(place,1))-1)<1  || board[# (i div ds_grid_width(placeable))+(real(string_char_at(place,1))-1)-1,(i mod ds_grid_width(placeable))+real(string_delete(place,1,1))] >= true) && string_char_at(canMove,2)  == "1"))  +  string((!((i div ds_grid_width(placeable))+(real(string_char_at(place,1))-1)>8  || board[# (i div ds_grid_width(placeable))+(real(string_char_at(place,1))-1)+1,(i mod ds_grid_width(placeable))+real(string_delete(place,1,1))] >= true) && string_char_at(canMove,3) == "1"))  }   } 
    place = string((real(string_char_at(place,1))) + ((string_char_at(canMove,3)  == "1") * keyboard_check_pressed(vk_right)) - ((string_char_at(canMove,2) == "1") * keyboard_check_pressed(vk_left))) + string(real(string_delete(place,1,1)) + (string_char_at(canMove,1)  == "1" && image_index mod (15-keyboard_check(vk_down)*13) == 0)) ; //only move down every 15th or always if holding down step. Checked using image index
    if ((real(string_char_at(place,1))-1) >= 0 && (real(string_char_at(place,1))-1)+ds_grid_width(placeable) <= 10 && ds_grid_get_max(board,(real(string_char_at(place,1))-1),real(string_delete(place,1,1)),(real(string_char_at(place,1))-1)+ds_grid_width(placeable)-1,real(string_delete(place,1,1))+ds_grid_width(placeable)-1) == 0 && keyboard_check_pressed(vk_up)) { //detect if you can rotate or not
        var rotatedGrid = ds_grid_create(ds_grid_width(placeable),ds_grid_width(placeable)); //rotate if able
        for (var i = 0; i < ds_grid_width(placeable)*ds_grid_width(placeable); i++)
            {rotatedGrid[# i div ds_grid_width(placeable), i mod ds_grid_width(placeable)] = placeable[# ds_grid_width(placeable) - (i mod ds_grid_width(placeable)) - 1, i div ds_grid_width(placeable)];}
        placeable = rotatedGrid; } //Replace the grid with a rotated one. We never remove the old one but it's only a SMALL memory leak...
    if (string_char_at(canMove,1)  == "0" && image_index mod (15-keyboard_check(vk_down)*13) == 0) { //add the pieces to field if you can move down and it's an update frame
        ds_grid_add_grid_region(board,placeable,0,0,ds_grid_width(placeable)-1,ds_grid_width(placeable)-1,(real(string_char_at(place,1))-1),real(string_delete(place,1,1)))
        ds_grid_clear(placeable,0)  }   } //clear the grid to show it's ready to be created again
```
