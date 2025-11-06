# Nodejs-project-
await collection.updateOne(
  { productID: "P001" },
  { $set: { price: 3.49 } }
);
const { MongoClient } = require('mongodb');

// MongoDB URI
const uri = "mongodb://localhost:27017"; // change this if using Atlas or remote DB
const client = new MongoClient(uri);

async function main() {
    try {
        // Connect to MongoDB
        await client.connect();
        console.log("Connected to MongoDB");

        // Select the database
        const database = client.db('productCatalog');

        // Select the collection
        const collection = database.collection('products');

        // Sample product document
        const newProduct = {
            productID: "P001",
            productName: "Mango",
            description: "A delicious tropical fruit.",
            category: "Fruit",
            price: 2.99,
            stockQuantity: 100,
            imageUrl: "https://example.com/mango.jpg"
        };

        // Insert product
        const insertResult = await collection.insertOne(newProduct);
        console.log(`New product inserted with the following id: ${insertResult.insertedId}`);

        // Retrieve all products
        const products = await collection.find({}).toArray();
        console.log("All Products:", products);
        
        // Retrieve a single product by productID
        const product = await collection.findOne({ productID: "P001" });
        console.log("Product Details:", product);
    } catch (error) {
        console.error("Error:", error);
    } finally {
        await client.close();
    }
}

main().catch(console.error);
