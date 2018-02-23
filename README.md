Tutorial from: https://hackernoon.com/learn-blockchains-by-building-one-117428612f46

Ensure you have **Python 3.6**

## Scenario

We run 3 nodes.

We register them to each other.

We create transactions and mine a some blocks on Node 3.

We run the Consensus algorithm to uniform the blockchain across the nodes.

> The consensus algorithm is simple: the longer chain is kept and erase the previous chain. 

### Run Node 1

    python3.6 blockchain.py --port 5000
    
### Run Node 2

    python3.6 blockchain.py --port 5001
    
### Run Node 3
 
    python3.6 blockchain.py --port 5002
    
### Register nodes

    curl -X POST \
      http://localhost:5000/nodes/register \
      -H 'Content-Type: application/json' \
      -d '{
        "nodes": [
                "http://127.0.0.1:5001",
                "http://127.0.0.1:5002"
                ]
    }'


    curl -X POST \
      http://localhost:5001/nodes/register \
      -H 'Content-Type: application/json' \
      -d '{
        "nodes": [
                "http://127.0.0.1:5000",
                "http://127.0.0.1:5002"
                ]
    }'
    
    curl -X POST \
      http://localhost:5002/nodes/register \
      -H 'Content-Type: application/json' \
      -d '{
        "nodes": [
                "http://127.0.0.1:5000",
                "http://127.0.0.1:5001"
                ]
    }'
    
### Create transaction on Node 3

    curl -X POST \
      http://localhost:5002/transactions/new \
      -H 'Content-Type: application/json' \
      -d '{
     "sender": "my address",
     "recipient": "someone else'\''s address",
     "amount": 5
    }'
    
### Mine some blocks on Node 3
Run this operation as many times as you want to create a new block.
Any transaction previously added is added to the new block.
The goal is to have a longer chain on this node.

    curl -X GET http://localhost:5002/mine
    
### Perform Consensus on the 3 nodes

    curl -X GET http://localhost:5000/nodes/resolve
    
    curl -X GET http://localhost:5001/nodes/resolve
        
    curl -X GET http://localhost:5002/nodes/resolve
    
### Check that the chain is the same for every nodes

    curl -X GET http://localhost:5000/chain
    
    curl -X GET http://localhost:5001/chain
        
    curl -X GET http://localhost:5002/chain