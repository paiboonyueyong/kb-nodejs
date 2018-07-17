# Mongodb Command

## Change database name
change mydb to sale_prediction

    db.copyDatabase("mydb","sale_prediction")
    use mydb
    db.dropDatabase();
    
## Delete single document from collection by _id

    db.[collection_name].deleteOne({"_id" : ObjectId("5b4d763957c44232e0685e7d")});
