#include<bits/stdc++.h>
using namespace std;
#define n 9


// This function is printing the grid
void printGrid(int grid[n][n])
{
    cout<<endl;
    for(int i=0;i<n;i++)
        {
            for(int j=0;j<n;j++)
                cout<<grid[i][j]<<' ';
            cout<<endl;
        }
    cout<<endl;
}

/*
    This function checks if the elements in sudoku are full or not ---  
    if there is vacant space then itreturns value of row and column where we can fill the values
*/
bool isFull(int grid[n][n],int &row, int &col)
{
    for(int i=0;i<n;i++){
        for(int j=0;j<n;j++)
        {
            if(grid[i][j] == 0)
            {
                row=i;
                col=j;
                return true;
            }
        }
    }
    return false;
}


//This function checks can we fill the element in vacant places in subarray
bool isSafeBox(int grid[n][n],int row, int col, int num){
    /*
    rowNum and colNum stores the value of row number and column number
    where we gonna fill the element respectively
    */
    int rowNum=row-(row%3);
    int colNum=col-(col%3);

    for(int i=0;i<3;i++){
        for(int j=0;j<3;j++)
        {
            if(grid[i+rowNum][j+colNum] == num)
                return false;
        }
    }
    return true;
}


// This function checks if there is any similar no. in row then it send false else true
bool isSafeRow(int grid[n][n], int row, int num)
{
    for(int i=0;i<n;i++)
    {
        if(grid[row][i] == num)
            return false;
    }
    return true;
}


// This function is similar to upper function as it checks if there is any similar no. in column then it sends false else true
bool isSafeCol(int grid[n][n], int col, int num)
{
    for(int i=0;i<n;i++)
    {
        if(grid[i][col] == num)
            return false;
    }
    return true;
}


/*
    This function checks there condtions if all conditions are satisfied then it returns true else false
    1. Is there any similar no. in the row
    2. Is there any similar no. in the column
    3. Is there any similar no. is subarray of 3*3
*/
bool isSafe(int grid[n][n],int row, int col, int num)
{
    if(isSafeBox(grid,row,col,num) && isSafeRow(grid,row,num) && isSafeCol(grid,col,num))
        return true;

    return false;
}


/* 
    This function checks all the conditons and then fills the element and if 
    it fills worng then then it backtrack and solve the problem by filling different numbers
*/
bool solveSudoku(int grid[n][n])
{
    int row,col;
    if(!isFull(grid,row,col))
        return true;

    for(int i=1;i<=n;i++){
        if(isSafe(grid,row,col,i)){
            grid[row][col]=i;

            //Here function calls it self and fills the no.
            if(solveSudoku(grid)){
                return true;
            }
            /*
                This is for back tracking if something wrong is 
                placed then when it go back it just removes the element and make is 0
            */
            grid[row][col]=0;
        }

    }
    return false;
}

int main()
{
    int grid[n][n] =
    {
                   {7,8,0,  0,6,2,  1,0,0},
                   {0,0,6,  4,1,0,  0,0,7},
                   {3,4,0,  0,8,0,  2,5,6},
                   {0,6,0,  0,0,1,  0,0,5},
                   {0,0,0,  0,0,0,  0,0,0},
                   {1,0,0,  8,0,0,  0,3,0},
                   {9,3,8,  0,4,0,  0,7,2},
                   {2,0,0,  0,3,8,  4,0,0},
                   {0,0,4,  2,7,0,  0,9,8}
    };

    printGrid(grid);

     solveSudoku(grid);

        printGrid(grid);

        return 0;
}