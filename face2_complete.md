# FACE2 --- MongoDB Practical Work

**Date:** 15 September 2026\
**Environment:** MongoDB Atlas + mongosh\
**Database/Prompt:** `PCEA24IT032`

## Part 2 ---aggrigation pipelines

### Create 5000 Products

const categories = [
    "Electronics",
    "Mobiles",
    "Laptops",
    "Home Appliances",
    "Fashion",
    "Books",
    "Sports",
    "Beauty",
    "Grocery",
    "Furniture"
];

const subCategories = {
    "Electronics": ["Headphones", "Smartwatch", "Camera", "Speaker"],
    "Mobiles": ["Android", "iPhone", "Feature Phone"],
    "Laptops": ["Gaming", "Business", "Student"],
    "Home Appliances": ["Refrigerator", "Washing Machine", "Microwave"],
    "Fashion": ["Men", "Women", "Kids"],
    "Books": ["Programming", "Fiction", "Education"],
    "Sports": ["Cricket", "Football", "Fitness"],
    "Beauty": ["Skincare", "Haircare", "Makeup"],
    "Grocery": ["Snacks", "Beverages", "Staples"],
    "Furniture": ["Chair", "Table", "Sofa"]
};

const brands = [
    "Samsung",
    "Apple",
    "Sony",
    "LG",
    "HP",
    "Dell",
    "Lenovo",
    "Nike",
    "Adidas",
    "Boat",
    "OnePlus",
    "AmazonBasics"
];

const cities = [
    "Chennai",
    "Coimbatore",
    "Bangalore",
    "Hyderabad",
    "Mumbai",
    "Delhi",
    "Pune",
    "Kochi",
    "Kolkata",
    "Ahmedabad"
];

const states = [
    "Tamil Nadu",
    "Karnataka",
    "Telangana",
    "Maharashtra",
    "Delhi",
    "Kerala",
    "West Bengal",
    "Gujarat"
];

const paymentMethods = [
    "UPI",
    "Credit Card",
    "Debit Card",
    "Net Banking",
    "Cash on Delivery"
];

const orderStatuses = [
    "Delivered",
    "Shipped",
    "Processing",
    "Cancelled",
    "Returned"
];

const sellers = [
    "RetailHub",
    "TechWorld",
    "MegaStore",
    "DigitalMart",
    "SmartShop",
    "PrimeRetail"
];

const tags = [
    "new",
    "popular",
    "trending",
    "premium",
    "budget",
    "best-seller",
    "discount",
    "limited-stock"
];


// Store documents temporarily
let documents = [];


// Generate 5000 documents
for (let i = 1; i <= 5000; i++) {

    const category =
        categories[Math.floor(Math.random() * categories.length)];

    const subCategory =
        subCategories[category][
            Math.floor(Math.random() * subCategories[category].length)
        ];

    const brand =
        brands[Math.floor(Math.random() * brands.length)];

    const city =
        cities[Math.floor(Math.random() * cities.length)];

    const state =
        states[Math.floor(Math.random() * states.length)];

    const paymentMethod =
        paymentMethods[
            Math.floor(Math.random() * paymentMethods.length)
        ];

    const orderStatus =
        orderStatuses[
            Math.floor(Math.random() * orderStatuses.length)
        ];

    const seller =
        sellers[
            Math.floor(Math.random() * sellers.length)
        ];


    const quantity =
        Math.floor(Math.random() * 5) + 1;

    const price =
        Math.floor(Math.random() * 95000) + 500;

    const discount =
        Math.floor(Math.random() * 51);

    const discountedPrice =
        Math.round(
            price - (price * discount / 100)
        );

    const rating =
        Number(
            (Math.random() * 4 + 1).toFixed(1)
        );

    const stock =
        Math.floor(Math.random() * 500);


    // Generate 2 random tags
    const selectedTags = [];

    for (let j = 0; j < 2; j++) {

        selectedTags.push(
            tags[Math.floor(Math.random() * tags.length)]
        );
    }


    // Random date between 2024 and 2026
    const startDate =
        new Date("2024-01-01").getTime();

    const endDate =
        new Date("2026-08-25").getTime();

    const randomDate =
        new Date(
            startDate +
            Math.random() * (endDate - startDate)
        );


    // Create document
    const product = {

        productId:
            "PROD" + String(i).padStart(5, "0"),

        productName:
            brand + " " + subCategory + " " + i,

        category: category,

        subCategory: subCategory,

        brand: brand,

        price: price,

        discountPercentage: discount,

        discountedPrice: discountedPrice,

        quantity: quantity,

        revenue:
            discountedPrice * quantity,

        rating: rating,

        reviewCount:
            Math.floor(Math.random() * 5000),

        stock: stock,

        inStock:
            stock > 0,


        seller: {

            name: seller,

            sellerRating:
                Number(
                    (Math.random() * 4 + 1).toFixed(1)
                )
        },


        customer: {

            customerId:
                "CUS" +
                String(
                    Math.floor(Math.random() * 1000) + 1
                ).padStart(4, "0"),

            city: city,

            state: state,

            age:
                Math.floor(Math.random() * 50) + 18
        },


        payment: {

            method: paymentMethod,

            transactionId:
                "TXN" +
                Math.random()
                    .toString(36)
                    .substring(2, 12)
        },


        orderStatus: orderStatus,

        orderDate: randomDate,

        tags: selectedTags,


        specifications: {

            warranty:
                (Math.floor(Math.random() * 3) + 1) +
                " years",

            color: [
                "Black",
                "White",
                "Blue",
                "Red",
                "Silver"
            ][Math.floor(Math.random() * 5)],

            weight:
                Number(
                    (Math.random() * 5 + 0.5).toFixed(2)
                )
        },


        isFeatured:
            Math.random() > 0.7,

        createdAt: new Date()
    };


    documents.push(product);


    // Insert every 500 documents
    if (documents.length === 500) {

        db.aggex.insertMany(documents);

        documents = [];
    }
}

// Insert remaining documents
if (documents.length > 0) {

    db.aggex.insertMany(documents);
}
print("5000 documents inserted successfully into aggex!");
use("PCEA24IT032")

db.aggex.aggregate([
{
    $match: {
        category: "Electronics"
    }
}
])
{
  _id: ObjectId("..."),
  productId: "PROD00015",
  productName: "Samsung Camera 15",
  category: "Electronics",
  price: 45000
}
db.aggex.aggregate([
{
    $group: {
        _id: "$category",
        totalProducts: { $sum: 1 }
    }
}
])
[
  { _id: "Electronics", totalProducts: 487 },
  { _id: "Mobiles", totalProducts: 503 },
  { _id: "Books", totalProducts: 491 }
]
db.aggex.aggregate([
{
    $group: {
        _id: "$category",
        totalRevenue: { $sum: "$revenue" }
    }
},
{
    $match: {
        totalRevenue: { $gt: 10000000 }
    }
}
])
[
  {
    _id: "Electronics",
    totalRevenue: 82000000
  },
  {
    _id: "Mobiles",
    totalRevenue: 76000000
  }
]
db.aggex.aggregate([
{
    $match: {
        orderStatus: "Delivered"
    }
},
{
    $group: {
        _id: "$category",
        totalRevenue: { $sum: "$revenue" }
    }
},
{
    $match: {
        totalRevenue: { $gt: 5000000 }
    }
},
{
    $sort: {
        totalRevenue: -1
    }
}
])
[
  {
    _id: "Electronics",
    totalRevenue: 52000000
  },
  {
    _id: "Mobiles",
    totalRevenue: 47000000
  },
  {
    _id: "Laptops",
    totalRevenue: 42000000
  }
]
 OUTPUTS:
 [
  { _id: "Delivered", totalOrders: 1025 },
  { _id: "Shipped", totalOrders: 984 },
  { _id: "Processing", totalOrders: 1001 },
  { _id: "Cancelled", totalOrders: 980 },
  { _id: "Returned", totalOrders: 1010 }
]