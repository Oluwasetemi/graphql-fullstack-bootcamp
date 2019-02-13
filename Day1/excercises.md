# Exercises Day1
Install Hasura Console on local environment

### Prerequisites
For running GraphQL Api locally [Install Docker](https://docs.docker.com/install/)

then run `./run-hasura-locally.sh`

This will bring up docker container with Hasura engine with connection to existing Postgres API
You can read more about Hasura [here](https://medium.com/open-graphql/effortless-real-time-graphql-api-with-serverless-business-logic-running-in-any-cloud-8585e4ed6fa3)

Access Hasura console on localhost:8080/console

- starwars api

[GraphiQL](https://graphql-bootcamp-swapi.herokuapp.com)

> Bonus you can add swapi custom server as remote schema in hasura console

[Remote schemas docs](https://docs.hasura.io/1.0/graphql/manual/remote-schemas/index.html#step-2-merge-remote-schema)


# Core principles


# Syntax and query language

1. What's wrong with this syntax?

```graphql
query {
  posts {
    timestamp,
    users {
      firstName
    }
  }
}
```

2. How to execute graphql request as curl

```json
curl -X POST \
  http://graphql-bootcamp-swapi.herokuapp.com/graphql \
  -H 'Content-Type: application/json' \
  -d '{
    "query": "query { allPlanets {planets {name\ndiameter} }}"
}'
```

# Queries

3. Get first 5 planets in Star Wars universe along with their name, diameter, rotation period, residents. For each resident display it's name, species, classification and spoken language. Also for each resident display vehicles that he used as well as in which movies they appeared

```graphql
query {
  allPlanets(first: 5) {
    planets {
      name
      diameter
      rotationPeriod
      residentConnection {
        residents {
          name
          species {
            classification
            language
          }
        }
      }
      vehicles
    }
  }

```graphql
query {
  ordered_posts:posts(order_by: {timestamp: asc}) {
    subject
    content
  }
}
```


# Mutations

5. Add new blog post

```graphql
mutation addnewpost {
  insert_posts(objects: {
    userId: "412968b4-7d3b-45e6-8890-f447b6d031fc",
    content: "Assignment Post is done halfway",
    subject: "Assignment is Good"
  }) {
    returning {
      subject
    }
  }
}
```

6. Add a new user using GraphQL Mutation

```graphql
mutation addNewUser {
  insert_users (objects: {
     firstName: "Oluwasetemi",
     lastName: "Stephen"
     profileId: "20a6b2d0-04e7-4240-b5a6-45f6a77dc4cb"
  }) {
    returning {
      firstName
      id
    }
  }
}
```

7. Create reusable insert mutation called addPost, which not only will insert a post, but create new user and profile

```graphql
mutation addPostUserProfile($subject: String!, $content: String) {
  insert_posts(objects: {
    subject: $subject
    content: $content
    user: {
      data: {
      	firstName: "Stephen"
        lastName: "Setemi"
        profile: {
          data: {
            avatarUrl: "https://avatars0.githubusercontent.com/u/10030028?v=3"
          }
        }
    	}
    }
  }) {
    returning {
      timestamp
      id
      userId
    }
  }
}
```


   > Hint: use variables


# Subscriptions
Return `n` most liked post where `n` can be provided from outside.


