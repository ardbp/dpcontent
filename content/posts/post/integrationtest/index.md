---
title: "Integration Test"
date: 2022-11-01 
lastmod: 2022-11-14
draft: false
tags: ["test", "integration test", "golang"]
categories: ["test"]
author: "Daxesh Panchal"

autoCollapseToc: true
# contentCopyright: '<a href="https://github.com/gohugoio/hugoBasicExample" rel="noopener" target="_blank">See origin</a>'

---

# **Integration Test part1**

## **What is Integration Test**

Integration test is kinds of test suite which performs end to end testing of your application. Let's try to understand with an example what includes in end to end testing.

For example if we have microservice which is serving APIs for customer's crud and maintain data in to postgres database. Now as part of Integration test we will cover following test cases for each api endpoint.

   * Success test case which validate you can create/update/delete/get customer resource with valid test data
   * Failure test cases
     *  Bad Request for Invalid data for all API endpoints
     *  Internal Server Error for the failure scenarios where service isn't reachable to postgres or any other scenarios where it can't process the request.
  

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

Let's write integration test for create endpoint

```

func TestCreateCustomerWithSuccessResponse(t *testing.T) {
	_, err := (getClient()).CreateCustomer(context.Background(), &api.Customer{Id: 1, Name: "test"})
	if err != nil {
		t.Errorf("failed to create customer with error: %v", err)
		t.Fail()
	}
}

func TestCreateCustomerWithBadRequestResponse(t *testing.T) {
	_, err := (getClient()).CreateCustomer(context.Background(), &api.Customer{})
	if err == nil {
		t.Errorf("failed to receive BadRequest response")
		t.Fail()
	}

	st, ok := status.FromError(err)
	if !ok {
		t.Errorf("Invalid response code")
		t.Fail()
	}

	if st.Code() != codes.InvalidArgument {
		t.Errorf("Invalid response code")
		t.Fail()
	}
}
```
This examples shows two different tests, where first test verify successful customer creation using correct data. 2nd test verify that customer create api return `InvalidArgument` response for invalid request data.

In both tests it uses generated gRPC client from protobuf defination, it has been provided using `getClient()` method. Here is the code for getClient.

```
func getClient() api.CustomerServiceClient {
	appURL := os.Getenv("APP_DSN")

	// connect to the grpc server
	conn, err := grpc.Dial(appURL, grpc.WithTransportCredentials(insecure.NewCredentials()))
	if err != nil {
		log.Print("did not connect:", err)
		return nil
	}

	return api.NewCustomerServiceClient(conn)
}
```
In this article we have talk about how to write basic integration test, In next article I will walk you through how to run this tests in local environment and on ci/cd using github actions.