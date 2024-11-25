********************************************************* web techologies *************************************************

slip9b

db.customers.insertMany([
  {
    "customer_id": "C001",
    "name": "Pranay",
    "address": "Vishrantwadi, Pune",
    "loans": [
      { "loan_id": "L001", "loan_type": "Home Loan", "loan_amount": 120000, "branch_city": "Vishrantwadi" },
      { "loan_id": "L002", "loan_type": "Car Loan", "loan_amount": 80000, "branch_city": "Pune" }
    ]
  },
  {
    "customer_id": "C002",
    "name": "Deepak Bhoine",
    "address": "Bopkhel, PCMC",
    "loans": [
      { "loan_id": "L003", "loan_type": "Education Loan", "loan_amount": 100000, "branch_city": "bhosari" }
    ]
  },
  {
    "customer_id": "C003",
    "name": "Aniket kh",
    "address": "Khadki, Pune",
    "loans": [
      { "loan_id": "L004", "loan_type": "Home Loan", "loan_amount": 200000, "branch_city": "Khadki" }
    ]
  },
  {
    "customer_id": "C004",
    "name": "Pooja M",
    "address": "Khadki, Pune",
    "loans": [
      { "loan_id": "L005", "loan_type": "Business Loan", "loan_amount": 50000, "branch_city": "Khadki" }
    ]
  },
  {
    "customer_id": "C005",
    "name": "Dinesh",
    "address": "Chinchwad, Pune",
    "loans": [
      { "loan_id": "L006", "loan_type": "Personal Loan", "loan_amount": 30000, "branch_city": "Pimpri" }
    ]
  },
  {
    "customer_id": "C006",
    "name": "Abhishek Landge",
    "address": "Lohegaon, Pune",
    "loans": [
      { "loan_id": "L007", "loan_type": "Car Loan", "loan_amount": 120000, "branch_city": "Dhanori" }
    ]
  },
  {
    "customer_id": "C007",
    "name": "Ravi Verma",
    "address": "Baner, Pune",
    "loans": [
      { "loan_id": "L008", "loan_type": "Home Loan", "loan_amount": 80000, "branch_city": "Pimpri" }
    ]
  },
  {
    "customer_id": "C008",
    "name": "Dhananjay Kulkarni",
    "address": "Wakad, Pune",
    "loans": [
      { "loan_id": "L009", "loan_type": "Business Loan", "loan_amount": 400000, "branch_city": "Pimpri" }
    ]
  },
  {
    "customer_id": "C009",
    "name": "Minal Jadhav",
    "address": "Aundh, Pune",
    "loans": [
      { "loan_id": "L010", "loan_type": "Education Loan", "loan_amount": 150000, "branch_city": "Pimpri" }
    ]
  },
  {
    "customer_id": "C010",
    "name": "Devendra Joshi",
    "address": "Hadapsar, Pune",
    "loans": [
      { "loan_id": "L011", "loan_type": "Personal Loan", "loan_amount": 100000, "branch_city": "Pimpri" }
    ]
  }
]);

--------------------------
queries

q1.

db.customers.find({ "name": { $regex: /^D/ } }, { "name": 1, "_id": 0 });

q2.

db.customers.find({ "loans.branch_city": "Pimpri" }, { "name": 1, "_id": 0 }).sort({ "name": -1 });

q3.

db.customers.aggregate([
  { $unwind: "$loans" },
  { $sort: { "loans.loan_amount": -1 } },
  { $limit: 1 }
]);

q4.

db.customers.updateOne(
  { "name": "Mr. Patil", "loans.loan_amount": { $gt: 100000 } },
  { $set: { "address": "New Address, Pune" } }
);

----------------------------------------------------------------------------
slip9a -HTML FORM

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Student Registration Form</title>
</head>
<body>
    <h2>Student Registration Form</h2>
    <form action="" method="">
        
        <label for="fullname">Full Name:</label>
        <input type="text" id="fullname" name="fullname" placeholder="Enter your full name" required><br><br>

        <label for="fathersname">Father's Name:</label>
        <input type="text" id="fathersname" name="fathersname" placeholder="Enter your father's name" required><br><br>

        <label for="email">Email Address:</label>
        <input type="email" id="email" name="email" placeholder="Enter your email" required><br><br>

        
        <label for="mobile">Mobile Number:</label>
        <input type="tel" id="mobile" name="mobile" placeholder="Enter your mobile number" pattern="[0-9]{10}" required><br><br>

        
        <label for="dob">Date of Birth:</label>
        <input type="date" id="dob" name="dob" required><br><br>

        
        <label for="gender">Gender:</label>
        <select id="gender" name="gender" required>
            <option value="">Select Gender</option>
            <option value="Male">Male</option>
            <option value="Female">Female</option>
            <option value="Other">Other</option>
        </select><br><br>

        <label for="address">Address:</label>
        <input type="text" id="address" name="address" placeholder="Enter your address" required><br><br>

        
        <label for="course">Select Course:</label>
        <select id="course" name="course" required>
            <option value="">Select Course</option>
            <option value="B.Sc">B.Sc</option>
            <option value="B.Com">B.Com</option>
            <option value="B.Tech">B.Tech</option>
            <option value="MCA">MCA</option>
            <option value="MBA">MBA</option>
        </select><br><br>

        
        <label for="search_college">Search Preferred College:</label>
        <input type="search" id="search_college" name="search_college" placeholder="Search for a college" required><br><br>

       
        <button type="submit">Register</button>
    </form>
</body>
</html>

***************************************************************************

slip10a

=> index.html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Trasition </title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="box"></div>
</body>
</html>
-------------------------------
=> style.css

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background-color: red;
}

.box {
    width: 100px;
    height: 100px;
    background-color: yellow;
    transition: transform 2s ease, background-color 2s ease 1s; 
}

.box:hover {
    transform: rotate(180deg);
    background-color: purple;
}

-----------------------------------------------------------------------------------------------------------------------

slip10b

customers collection
---------------------
[
  {
    "_id": 1,
    "name": "Pooja Mahendra",
    "city": "Pune",
    "purchases": [
      { "product": "Smartwatch", "purchase_date": "2023-08-15", "amount": 55000 }
    ]
  },
  {
    "_id": 2,
    "name": "Pranay Barathe",
    "city": "Mumbai",
    "purchases": [
      { "product": "Laptop", "purchase_date": "2023-08-15", "amount": 45000 }
    ]
  },
  {
    "_id": 3,
    "name": "Abhishek Landge",
    "city": "Pune",
    "purchases": [
      { "product": "Smartphone", "purchase_date": "2023-08-16", "amount": 60000 }
    ]
  },
  {
    "_id": 4,
    "name": "Aniket kh",
    "city": "Delhi",
    "purchases": [
      { "product": "Tablet", "purchase_date": "2023-08-10", "amount": 20000 }
    ]
  },
  {
    "_id": 5,
    "name": "Atharv tekawale",
    "city": "Bangalore",
    "purchases": [
      { "product": "Headphones", "purchase_date": "2023-08-14", "amount": 30000 }
    ]
  }
]



--------------
products collection

[
  {
    "_id": 1,
    "name": "Smartphone",
    "brand": "Iphone",
    "warranty_period": 1,  
    "rating": 4.5
  },
  {
    "_id": 2,
    "name": "Laptop",
    "brand": "Dell",
    "warranty_period": 2,  
    "rating": 4.0
  },
  {
    "_id": 3,
    "name": "Smartwatch",
    "brand": "Noise",
    "warranty_period": 1,  
    "rating": 4.7
  },
  {
    "_id": 4,
    "name": "Headphones",
    "brand": "OnePlus Nord 2R",
    "warranty_period": 1,  
    "rating": 4.3
  },
  {
    "_id": 5,
    "name": "Tablet",
    "brand": "Samsung",
    "warranty_period": 2,  
    "rating": 3.8
  }
]

---------------
brand collection

[{
  "_id": 1,
  "name": "Noise",
  "rating": 4.6
},
{
  "_id": 2,
  "name": "iphone",
  "rating": 3.9
},
{
  "_id": 3,
  "name": "samsung",
  "rating": 4.1
}
]


--------------
Queries

q1.

db.products.find(
  { "warranty_period": 1 }, 
  { "name": 1, "_id": 0 }
)

q2.

db.customers.find(
  { "purchases.purchase_date": "2023-08-16" },
  { "name": 1, "_id": 0 }
)

q3.

db.brand.find().sort({ "rating": -1 }).limit(1).forEach(function(brand) {
  db.products.find({ "brand": brand.name }).forEach(function(product) {
    print(product.name + " - " + brand.name)
  })
})

q4.

db.customers.find(
  { "city": "Pune", "purchases.amount": { $gt: 50000 } },
  { "name": 1, "_id": 0 }
)


****************************************************************************





