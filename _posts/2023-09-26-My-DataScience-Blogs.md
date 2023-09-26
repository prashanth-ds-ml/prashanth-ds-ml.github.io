---
layout: page
title: "My Data Science Blogs"
---

# Essential Tensor Manipulations (Part 1)

* Welcome to the first part of our series on tensor manipulations using Python and PyTorch! 🧠🔥 In this blog post, we'll explore the foundational aspects of tensor manipulation, providing you with a detailed and comprehensive understanding of each operation. Let's dive in and demystify the world of tensors.

### 1. Tensor Creation

* lets create a tensor filled with zeros
* lets create a 3 X 4 zeros tensor
* In python `()` are used to group expressions and pass arguments to functions.
* we create zeros tensor using `torch.zeros()` we pass shape of the tensor as a tuple of dimensions
* Tuples are ordered collection of elements, enclosed in `()`
* In the example `torch.zeros((3, 4))`, the tuple `(3, 4)` indicates that the tensor should have 3 rows and 4 columns, resulting in a 3x4 matrix filled with zeros.


```python
import torch
zeros_tensor = torch.zeros((3,4))
zeros_tensor
```




    tensor([[0., 0., 0., 0.],
            [0., 0., 0., 0.],
            [0., 0., 0., 0.]])




```python
# Create a tensor filled with ones
ones_tensor = torch.ones((3, 4))
ones_tensor
```




    tensor([[1., 1., 1., 1.],
            [1., 1., 1., 1.],
            [1., 1., 1., 1.]])




```python
# Create a tensor with random values from a uniform distribution (0 to 1)
random_tensor = torch.rand((3, 4))
random_tensor
```




    tensor([[0.7679, 0.0413, 0.6213, 0.2835],
            [0.9564, 0.4476, 0.5172, 0.4087],
            [0.5153, 0.8642, 0.8715, 0.4116]])




```python
# Create a tensor with random values from a standard normal distribution
normal_random_tensor = torch.randn((3, 4))
normal_random_tensor
```




    tensor([[ 0.4850, -0.3211, -0.1980,  1.0192],
            [-0.4839, -0.0429, -1.3275, -0.6466],
            [ 0.0936, -0.0401,  0.2062,  0.3264]])



* Wrong ways to create a tensor


```python
# Incorrect usage with square brackets (will create a 1-dimensional tensor)
zeros_tensor = torch.zeros[3, 4]  # Creates a tensor with shape (2,)
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    Cell In[39], line 2
          1 # Incorrect usage with square brackets (will create a 1-dimensional tensor)
    ----> 2 zeros_tensor = torch.zeros[3, 4]  # Creates a tensor with shape (2,)


    TypeError: 'builtin_function_or_method' object is not subscriptable


### 2. Tensor Reshaping


```python
original_tensor = torch.tensor([[1, 2, 3], [4, 5, 6]])
original_tensor
```




    tensor([[1, 2, 3],
            [4, 5, 6]])




```python
# Reshape the tensor to have 3 rows and 2 columns
reshaped_tensor = original_tensor.view(3, 2)
reshaped_tensor
```




    tensor([[1, 2],
            [3, 4],
            [5, 6]])




```python
# Add a new dimension to the tensor
expanded_tensor = original_tensor.unsqueeze(0)  # Adds a batch dimension
expanded_tensor
# if you observe clearly there is extra pair of [] in the output which shows us extra dimension is added
```




    tensor([[[1, 2, 3],
             [4, 5, 6]]])




```python
# Remove the added dimension
# here we observe that extra dimension gets squeezes/removed
squeezed_tensor = expanded_tensor.squeeze(0)
squeezed_tensor
```




    tensor([[1, 2, 3],
            [4, 5, 6]])



### 3. Tensor Arithmetic Operations

* Tensor arithmetic involves performing element-wise operations on tensors, such as addition, subtraction, multiplication, and division


```python
# Create tensors
tensor1 = torch.tensor([[1, 2], [3, 4]])
tensor2 = torch.tensor([[5, 6], [7, 8]])

# Addition
add_result = tensor1 + tensor2

# Subtraction
sub_result = tensor1 - tensor2

# Multiplication
mul_result = tensor1 * tensor2

# Division
div_result = tensor1 / tensor2

print("Addition:")
print(add_result)
print("Subtraction:")
print(sub_result)
print("Multiplication:")
print(mul_result)
print("Division:")
print(div_result)

```

    Addition:
    tensor([[ 6,  8],
            [10, 12]])
    Subtraction:
    tensor([[-4, -4],
            [-4, -4]])
    Multiplication:
    tensor([[ 5, 12],
            [21, 32]])
    Division:
    tensor([[0.2000, 0.3333],
            [0.4286, 0.5000]])


### 4. Broadcasting

#### 4.1 Broadcasting Scalars to Tensors


```python
# Create a tensor
tensor = torch.tensor([[1, 2, 3], [4, 5, 6]])

# Add a scalar to the tensor
result = tensor + 10

print("Original Tensor:")
print(tensor)
print("Shape of Original Tensor:", tensor.shape,'\n')
print("Result after Broadcasting:")
print(result,'\n')
print("Shape of Result:", result.shape)
```

    Original Tensor:
    tensor([[1, 2, 3],
            [4, 5, 6]])
    Shape of Original Tensor: torch.Size([2, 3]) 
    
    Result after Broadcasting:
    tensor([[11, 12, 13],
            [14, 15, 16]]) 
    
    Shape of Result: torch.Size([2, 3])



```python
# Create tensors
tensor1 = torch.tensor([[1, 2], [3, 4]])
tensor2 = torch.tensor([[10], [20]])

# Broadcasting tensor2 to tensor1
broadcast_result = tensor1 + tensor2

print("Broadcasting with Different Dimensions:")
print(broadcast_result)

```

    Broadcasting with Different Dimensions:
    tensor([[11, 12],
            [23, 24]])


#### 4.2 Broadcasting Vectors to Matrices


```python
# Create a matrix and a vector
matrix = torch.tensor([[1, 2, 3], [4, 5, 6]])
vector = torch.tensor([10, 20, 30])

# Add the vector to each row of the matrix
result = matrix + vector

print("Original Matrix:")
print(matrix)
print("Shape of Original Matrix:", matrix.shape,'\n')
print("Vector:")
print(vector)
print("Shape of Vector:", vector.shape,'\n')
print("Result after Broadcasting:")
print(result)
print("Shape of Result:", result.shape)

```

    Original Matrix:
    tensor([[1, 2, 3],
            [4, 5, 6]])
    Shape of Original Matrix: torch.Size([2, 3]) 
    
    Vector:
    tensor([10, 20, 30])
    Shape of Vector: torch.Size([3]) 
    
    Result after Broadcasting:
    tensor([[11, 22, 33],
            [14, 25, 36]])
    Shape of Result: torch.Size([2, 3])


#### 4.3 Broadcasting Along Multiple Dimensions


```python
# Create a 3D tensor and a 2D matrix
tensor = torch.tensor([[[1, 2], [3, 4]], [[5, 6], [7, 8]]])
matrix = torch.tensor([[10, 20], [30, 40]])

# Add the matrix to each element of the tensor
result = tensor + matrix

print("Original Tensor:")
print(tensor)
print("Shape of Original Tensor:", tensor.shape,'\n')
print("Matrix:")
print(matrix)
print("Shape of Matrix:", matrix.shape,'\n')
print("Result after Broadcasting:")
print(result)
print("Shape of Result:", result.shape)

```

    Original Tensor:
    tensor([[[1, 2],
             [3, 4]],
    
            [[5, 6],
             [7, 8]]])
    Shape of Original Tensor: torch.Size([2, 2, 2]) 
    
    Matrix:
    tensor([[10, 20],
            [30, 40]])
    Shape of Matrix: torch.Size([2, 2]) 
    
    Result after Broadcasting:
    tensor([[[11, 22],
             [33, 44]],
    
            [[15, 26],
             [37, 48]]])
    Shape of Result: torch.Size([2, 2, 2])


#### 4.4 Broadcasting with Dimension Expansion


```python
# Create tensors with different dimensions
tensor1 = torch.tensor([[1, 2], [3, 4]])
tensor2 = torch.tensor([10, 20])

# Broadcast tensor2 to match the shape of tensor1
result = tensor1 + tensor2[:, None]

print("Tensor1:")
print(tensor1)
print("Shape of Tensor1:", tensor1.shape,'\n')
print("Tensor2:")
print(tensor2)
print("Shape of Tensor2:", tensor2.shape,'\n')
print("Result after Broadcasting:")
print(result)
print("Shape of Result:", result.shape)

```

    Tensor1:
    tensor([[1, 2],
            [3, 4]])
    Shape of Tensor1: torch.Size([2, 2]) 
    
    Tensor2:
    tensor([10, 20])
    Shape of Tensor2: torch.Size([2]) 
    
    Result after Broadcasting:
    tensor([[11, 12],
            [23, 24]])
    Shape of Result: torch.Size([2, 2])


### 5. Tensor Concatenation


```python
tensor1 = torch.tensor([[1, 2], [3, 4]])
tensor2 = torch.tensor([[5, 6]])

# Concatenate tensors along the specified dimension (0 for rows, 1 for columns)
concatenated_tensor = torch.cat((tensor1, tensor2), dim=0)
concatenated_tensor
```




    tensor([[1, 2],
            [3, 4],
            [5, 6]])




```python
tensor1 = torch.tensor([[1, 2], [3, 4]])
tensor2 = torch.tensor([[5, 6]])

# Transpose tensor2 to match the number of rows in tensor1 before concatenation
tensor2_transposed = tensor2.t()

# Concatenate tensors along the specified dimension (0 for rows, 1 for columns)
concatenated_tensor = torch.cat((tensor1, tensor2_transposed), dim=1)
concatenated_tensor

```




    tensor([[1, 2, 5],
            [3, 4, 6]])




```python
x = torch.tensor([[1,2,3,4,5,6],[4,5,8,6,2,3]])
y = torch.tensor([[4,5,6,8,9,7],[1,2,3,4,5,6]])

stacked_tensor = torch.stack((x,y),dim = 1)

print(stacked_tensor)
```

    tensor([[[1, 2, 3, 4, 5, 6],
             [4, 5, 6, 8, 9, 7]],
    
            [[4, 5, 8, 6, 2, 3],
             [1, 2, 3, 4, 5, 6]]])




## Blog Post 2

This is the content of my second data science blog post.

// Add more blog posts as needed...
