# go-graphql

This is a study project aimed at gaining a basic understanding of GraphQL and how to create and perform queries using the Go programming language as the backend. The project serves as a beginner's guide to GraphQL and demonstrates its implementation in a Go-based server.
Table of Contents

* Introduction
* Technologies Used
* Getting Started
* Create tables in Sqlite3
* Creating a GraphQL Schema
* Running the Server
* Making Queries

## Introduction

GraphQL is an open-source query language for APIs and a runtime for executing those queries with existing data. It provides a more efficient and flexible alternative to traditional RESTful APIs by allowing clients to specify the exact data requirements they need from the server.

This project focuses on understanding the basics of GraphQL and how it can be implemented in a Go backend server. We will cover the process of creating a GraphQL schema, defining query resolvers, setting up a SQLite database, and making queries to retrieve data.
Technologies Used

    Go: The backend language used to develop the GraphQL server.
    GraphQL-Go: A Go implementation of GraphQL, used to create the server and handle GraphQL operations.
    SQLite3: A lightweight database engine used for storing and retrieving data.

## Getting Started

To get started with the project, follow these steps:

 1.   Clone the repository:


```bash
git clone https://github.com/kleber-yokota/go-graphql
```
 2.   Install the necessary dependencies:

```bash

cd go-graphql
go mod download
```
 3.   Set up the SQLite database:

```bash
# Assuming you have SQLite installed
sqlite3 data.db
```
 4.   Start exploring the project!

## Create Tables in Sqlite3

```sqlite3
CREATE TABLE categories (id string, name string, description string);
CREATE TABLE courses (id string, name string, description string, category_id string);
```


## Creating a GraphQL Schema

The GraphQL schema defines the types of data that can be queried and the relationships between them. In the schema.go file, you can define your schema using the GraphQL Schema Definition Language (SDL). Define types, queries, and mutations according to your application's needs.

Here's an example of a basic schema with Category and Course types:

```schema.graphqls
type Category {
  id: ID!
  name: String!
  description: String
  courses: [Course!]
}

type Course {
  id: ID!
  name: String!
  description: String
  Category: Category!
}

input NewCategory {
  name: String!
  description: String
}

input NewCourse {
  name: String!
  description: String
  categoryId: ID!
}

type Query {
  categories : [Category!]!
  courses: [Course!]!
}

type Mutation {
  createCategory(input: NewCategory!): Category!
  createCourse(input: NewCourse!): Course!
}

```

## Running the Server

To start the GraphQL server, run the following command:

```bash
go run cmd/server/server.go
```

The server will start running on a specified port (e.g., http://localhost:8080).


## Making Queries

```graphql
mutation createCategory {
  createCategory(input: {name: "Tecnologia",
  description:"Curso de tecnologia"}){
    id
    name
    description
  }
}

mutation createCourse{
  createCourse(input: {name: "Full Cycle", description: "the best", categoryId:<category id>})
    {
    id
    name
  }
}

query queryCategories {
  categories{
    id
    description
  }
}

query queryCategoriesWithCourses {
  categories{
    id
    name
    courses{
      id
      name
    }
  }
}

query queryCoursesWithCategory{
  courses{
    id
    name
    description
    Category{
      id
      name
      description
    }
  }
}
```