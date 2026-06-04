Keep references to a working variable and a vector which has all the answer.
Each recursive call mutates that working variable. Once base-case is reached, push a copy of the working variable to the answer vector. 
The function itself returns void. 