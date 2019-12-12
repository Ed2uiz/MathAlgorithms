
# Row Reduced Echelon Form in Python

```python
import numpy as np
```



##### RREF review 

```python
    J-G algorithm (https://www.di-mgt.com.au/matrixtransform.html)
    # set j to 1
    # for each row i from 1 to n
        # While column j has all zero elements, set j←j+1. If j>m return A. 
        # If element aij is zero, then interchange row i with a row x>i that has axj≠0.
        # Divide each element of row i by aij, thus making the pivot aij equal to one.
        # For each row k from 1 to n, with k≠i, subtract row i multiplied by akj from row k.
    # return transformed matrix A.
```

##### Create 200x200 matrix with random integers from -100 to 100

```python
rref_randomMat = np.random.randint(-100, 100, size=(200,200)) 
rref_randomMat
```




    array([[ 80,  45, -63, ...,  37,  79, -42],
           [-22,  84, -37, ...,  62,  64, -55],
           [-53,   9,  64, ...,  89, -40, -19],
           ...,
           [ -4,  -4,  19, ...,  53, -14,  55],
           [ -6, -21, -72, ..., -93,  40,   4],
           [ 19,  53,  19, ...,  18, -62,  59]])



##### Function to compute RREF

```python
def RREF(mat):
    rowSize = np.size(mat, 0)
    colSize = np.size(mat, 1)
    i = 0
    j = 0

    for i in range(0, rowSize): 
        for j in range(0, colSize): 
            if mat[i][j] == 0:  # scan rows with "leading 0s"
                scanRow = i 
                temp = mat[i] 
                # while there is a "leading zero" for a row, scan next row
                while scanRow < rowSize and mat[scanRow][j] == 0:  
                    scanRow += 1 
                # if there are leading zeros in every row of first column
                # move to next column (j)
                if scanRow == rowSize:  
                    j += 1
                    continue
                
                # Swap rows
                mat[i] = mat[scanRow]  # Scanned row without "leading zero"
                mat[scanRow] = temp   # Interchange rows
            
            # Force leading 1 by dividng leading integer in row (pivot)
            mat[i] = [k / mat[i][j] for k in mat[i]]  
            
            # Scan remaining rows to force remaining pivots 
            for newRow in range(0, rowSize):   
                if newRow == i: 
                    continue  # Iterate for all rows in matrix
                if mat[newRow][j] != 0:  # Enforce leading zero structure
                    # Force leading ones in rows by multiplying newrow 
                    mat[newRow] = [y - mat[newRow][j]*x for (x, y) in zip(mat[i], mat[newRow])]  
            i += 1
            j += 1
            
            if i >= rowSize or j >= colSize:  # Exit loop once mat dims reached
                break
    return mat
```


```python
RREF(rref_randomMat)
```




    array([[1, 0, 0, ..., 0, 0, 0],
           [0, 1, 0, ..., 0, 0, 0],
           [0, 0, 1, ..., 0, 0, 0],
           ...,
           [0, 0, 0, ..., 1, 0, 0],
           [0, 0, 0, ..., 0, 1, 0],
           [0, 0, 0, ..., 0, 0, 1]])


