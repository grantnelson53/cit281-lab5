## Lab 5

In this lab, we continued our knowledge about Fastify and used client-server communication

### Source Code

    // Require the Fastify framework and instantiate it
    const fastify = require("fastify")();
    // Handle GET verb for / route using Fastify
    // Note use of "chain" dot notation syntax
    const students = [
      {
        id: 1,
        last: "Last1",
        first: "First1",
      },
      {
        id: 2,
        last: "Last2",
        first: "First2",
      },
      {
        id: 3,
        last: "Last3",
        first: "First3",
      },
    ];

    //Student Route 
    fastify.get("/cit/student", (request, reply) => {
      reply
        .code(200)
        .header("Content-Type", "application/json; charset=utf-8")
        .send(students);
    });

    //Student ID Route
    fastify.get("/cit/student/:id", (request, reply) => {
        console.log(request);

        let studentIDFromClient = request.params.id;
        console.log(studentIDFromClient);

        let studentRequestedFromClientID = null;

        for (studentInArray of students) {
            if (studentInArray.id == studentIDFromClient) {
                studentRequestedFromClientID = studentInArray;
                break;
            }
        }
     if (studentRequestedFromClientID != null) {
      reply
        .code(200)
        .header("Content-Type", "application/json; charset=utf-8")
        .send(studentRequestedFromClientID);
        }
    else {
          reply
            .code(404)
            .header("Content-Type", "text/html; charset=utf-8")
            .send("<h1>404 Not Found</h1>");
        }
    });

    //Unmatched Route
    fastify.get("*", (request, reply) => {
      reply
        .code(200)
        .header("Content-Type", "text/html; charset=utf-8")
        .send("<h1>404 Not Found</h1>");
    });

    //Add  Studnet Route
    fastify.post("/cit/student/add", (request, reply) => {
        console.log(request);
        let userData = JSON.parse(request.body)
        console.log(userData);

      reply
        .code(200)
        .header("Content-Type", "application/json; charset=utf-8")
        .send("<h1>At 'ADD STUDENT' Route</h1>");
    });



    // Start server and listen to requests using Fastify
    const listenIP = "localhost";
    const listenPort = 8080;
    fastify.listen(listenPort, listenIP, (err, address) => {
      if (err) {
        console.log(err);
        process.exit(1);
      }
      console.log(`Server listening on ${address}`);
    });
