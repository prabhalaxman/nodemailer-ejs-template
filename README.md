# nodemailer-ejs-template
Here is code snipet to render html file using ejs and send as template using nodemailer
Sample html file looks like 


<!DOCTYPE html>
<html lang="en">

<head>
  <title>Mail</title>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.0/css/bootstrap.min.css">
  <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.7.0/css/all.css">
  <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
  <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.0/js/bootstrap.min.js"></script>

  <style>
    table,
    th,
    td {
      border: 1px solid black;
      border-collapse: collapse;
    }

    .headings {
      background-color: grey;
    }

    .dualButtons {
      text-align: center;
    }

    .btn-success {
      background-color: #009900;
      border-color: #009900;
    }

    .btn-danger {
      background-color: #ff0000;
      border-color: #ff0000;
    }
  </style>
</head>

<body>

  <div class="container">
    <div class="row">
      <h2><i class="fa fa-envelope"></i> Organization name</h2>
      <p>Hi ,</p>
      <p><%= userName %> - <%= userCategory %> has raised the purchase for the <%= shipName %> with the following
        details.</p>
      <p> Kindly accept or reject the purchase.</p>

      <table style="width:100%">
        <tr class="headings">
          <th>S.No </th>
          <th>Name</th>
          <th>Type</th>
          <th>Grade</th>
          <th>Qty</th>
          <th>Price</th>

        </tr>
        <% for(var i=0; i < data.length; i++) { %>
        <tr>
          <td><%= i+1 %></td>
          <td><%= data[i].name %></td>
          <td><%= data[i].type %></td>
          <td><%= data[i].grade %></td>
          <td><%= data[i].qty %></td>
          <td><%= data[i].price %></td>
        </tr>
        <% } %>
      </table>


      <br>

      <div class="dualButtons">


        <a class="btn btn-success" href=<%= acceptUrl %>> Accept</a>
        <a class="btn btn-danger" href=<%= rejectUrl %>>Reject</a>

      </div>
      <br>
      <p>If you want to verify all the purchase, Kindly Login using the below link</p>
      <a href="http:localhost:3000/login" class="btn btn-success">Login</a>

    </div>
  </div>

</body>

</html>



And sample code snipet to send mail using nodemailer


try {

        var acceptUrl = "http://localhost:3000/hbs_sample/" + userId + "/" + purchaseId
        var rejectUrl = "http://localhost:3000/hbs_sample/" + userId + "/" + purchaseId
        var user = {
            userName: "Jack",
            userCategory: 'Captain',
            shipName: 'Test ship',
            firstName: 'John', lastName: 'Doe', data: [
                { name: "Xyz ", type: "Oil1", grade: "DMA", qty: "10", price: "1000" },
                { name: "Xyz ", type: "Oil2", grade: "DMC", qty: "20", price: "3000" },
                { name: "Xyz ", type: "Oil3", grade: "DMX", qty: "10", price: "2000" },
            ],
            acceptUrl: acceptUrl,
            rejectUrl: rejectUrl
        };
        var subject = ejs.render('Hello <%= firstName %>', user);


        var templateString = fs.readFileSync('html file path', 'utf-8');

        // var template = hbs.compile(templateString);

        var text = ejs.render(templateString, user);

        var transporter = nodemailer.createTransport({
            service: 'Gmail',
            auth: {
                user: '<Your gmail>',
                pass: '<Password>'
            }
        });

        var maillist = [
            'xxxxxx@gmail.com',
            'yyyyy@gmail.com'
          ];

        // Message object
        let message = {
            from: 'from mail id',
            // Comma separated list of recipients
            to: maillist,

            // Subject of the message
            subject: subject,

            // plaintext body
            // text: template("http:localhost:3000"),
            html: text
          
        };

        console.log('node mailer msg ' + message)
        transporter.sendMail(message, (error, info) => {
            if (error) {
                console.log('send mail error ' + error)
                return process.exit(1);
            }
            console.log('mail msg ' + message)
            // only needed when using pooled connections
            transporter.close();
        });


    } catch (err) {
        // writeErrorInLogFile("Error Log mail sending ", errorLog, config.error);
        // reject(err);
        // res.json({ "status": "Failure", "Message": "Failed to update Port Info" });
        console.log('send mail error ' + err)
    } finally {
    }








