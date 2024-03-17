# Introduction

This package is designed to simplify error handling in backend projects by replacing multiple instances of try-catch blocks with just one function **asyncHandler**. It also includes an API response utility (**ApiResponse**) that simplifies the process of sending responses to clients. With the API-Response function, you only need to provide the status code and message, and optionally, the data you want to send. Additionally, the package provides an API-Error function (**ApiError**) to help you manage and handle error messages more efficiently.


# Getting started
**Installation**
This package can be installed using `npm`

    npm i trycatch-alter

**Importing functions**

import asyncHandler function

    const { asyncHandler } =  require('trycatch-alter');
    
import ApiResponse function

    const { ApiResponse } =  require('trycatch-alter');

   
import ApiError
  
    const { ApiError } =  require('trycatch-alter');
Or you import all function in one line

    const { ApiError, ApiResponse, asyncHandler } =  require('trycatch-alter');

## How to use this function

**For asyncHandler**

    exports.example = asyncHandler(async (req, res) => {
	    // Write your backend logic here
	    // No need to write a try-catch block; asyncHandler will handle errors
    });

**For Api Response**

    new ApiResponse(statusCode, Data, "your message for response");

**For Api Error**

    throw  new ApiError(statusCode, "your message for the error");

## Example

**Example 1**

Now, We understand how to use this npm library through a simple example, like creating a todo app, where I can demonstrate how to use it in my `createTodo` file.

    // Import the Todo model  
    const  Todo = require("../models/Todo"); 
    
    // Import necessary functions from the trycatch-alter library  
    const { ApiError, ApiResponse, asyncHandler } = require('trycatch-alter');
    
    // Define the route handler for creating a new todo 
    exports.createTodo = asyncHandler(async (req, res) => {
     
	    // Extract the title and description from the request body  
	    const { title, description } = req.body; 
	    
	    // Check if both title and description are provided  
	    if (!title || !description) { 
		    throw  new  ApiError(400, "Please provide both a title and a description"); 
		}
		
		// Create a new todo object and insert it into the database
		const todo = await Todo.create({ title, description });
		
		// Send a JSON response with a success message
		res.status(200)
		.json(new ApiResponse(200, todo, "Todo created successfully"));
		});

**Example 2**

    // Import the Todo model and necessary functions from the trycatch-alter library
    const Todo = require("../models/Todo");
    const { ApiError, ApiResponse, asyncHandler } = require('trycatch-alter');
    
	    // Route handler to fetch all todo items
	    exports.getTodo = asyncHandler(async (req, res) => {
		    
		    // Fetch all todo items from the database
		    const todos = await Todo.find({});
    
		    // Respond with the fetched todo items
		    res.status(200).json(
		        new ApiResponse(200, todos, "All todo data successfully fetched")
		        );
		});


	    // Route handler to fetch a todo item by ID
	    exports.getTodoById = asyncHandler(async (req, res) => {
		    
		    // Extract the todo item ID from the request parameters
		    const id = req.params.id;
    
		    // Find the todo item in the database based on the ID
		    const todo = await Todo.findById({ _id: id });

		    // If no todo item is found with the given ID, throw an error
		    if (!todo) {
		        throw new ApiError(404, "No data found with the given ID");
		    }

		    // Respond with the fetched todo item
		    res.status(200).json(
		        new ApiResponse(200, todo, `Todo item with ID ${id} successfully fetched`)
		    );
	    });



## License

This project is licensed under the MIT license.

# Disclaimer

Please ensure that you follow the correct installation and usage instructions for this library. Improper usage may lead to unexpected behavior or errors. Always refer to the official documentation for the most up-to-date information.


# Keywords

 1. trycatch-alter   
 2. ApiResponse 
 3. ApiError






