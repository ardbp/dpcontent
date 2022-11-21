---
title: "Integration Test"
date: 2022-11-20T15:43:48+08:00  
lastmod: 2022-11-20T15:43:48+08:00
draft: false
tags: ["test", "integration test", "golang"]
categories: ["test"]
author: "Daxesh Panchal"

autoCollapseToc: true
# contentCopyright: '<a href="https://github.com/gohugoio/hugoBasicExample" rel="noopener" target="_blank">See origin</a>'

---

# **Integration Test**

## **What is Integration Test**

Integration test is kinds of test suite which performs end to end testing of your application. Let's try to understand with an example what includes in end to end testing.

For example if we have microservice which is serving APIs for customer's crud and maintain data in to postgres database. Now as part of Integration test we will cover following test cases for each api endpoint.

   * Success test case which validate you can create/update/delete/get customer resource with valid test data
   * Failure test cases
     *  Bad Request for Invalid data for all API endpoints
     *  Internal Server Error for the failure scenarios where service isn't reachable to postgres or any other scenarios where it cann't process the request.
  

## **How to write integration test**

As integration test will be testing end-to-end functionlity of your application we should testing our application same way how end user is going to use it.

Let's take a this example where I have Microservice which provides gRPC APIs for customer create and get endpoints. Here is proto defination for this service. 

```
// CustomerService will provide two gRPC endpoint 
service CustomerService {
  // GetCustomer will retrieve customer
  rpc GetCustomer (GetCustomerRequest) returns (Customer){};
  // CreateCustomer will allow to create customer
  rpc CreateCustomer (Customer) returns (CustomerResponse){};
}

// GetCustomerRequest customer request payload
message GetCustomerRequest {
  int64 id = 1;
}

message Customer {
  string name = 1;
  int64 id = 2;
}

// CustomerResponse customer response payload
message CustomerResponse{}
```

So when end user will be using this service they will interect with either create or get RPC endpoint, Similarly we will be writing the test which will call both endpoints using generated gRPC client from above protobuf.

