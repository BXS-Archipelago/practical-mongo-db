Connect to Mongo CLI on Gitpod
navigate to your MongoDB Clusters Sandbox
click "Connect" button
select "Connect with the mongo shell"
select "I do not have the mongo shell installed"
choose option: "Run your connection string in your command line"
mongo "mongodb+srv://<CLUSTER-NAME>.mongodb.net/<DBname>" --username <USERNAME>
replace all <angle-bracket> keys with your own data
enter password *(will not echo ******** on screen)
Clear screen in Mongo Shell:
cls
Show all database collections:
show collections
Assign collection to variable 'coll':
coll = db.collection_name
Insert data to collection:
coll.insert({
    first: "john",
    last: "lennon",
    dob: "09/10/1940",
    gender: "m",
    hair_color: "brown",
    occupation: "beatle",
    nationality: "british"
});
coll.insert({
    first: "eve",
    last: "ryan",
    dob: "19/09/1992",
    gender: "f",
    hair_color: "pink",
    occupation: "developer",
    nationality: "irish"
});
coll.insert({
    first: "martha",
    last: "fenton",
    dob: "15/05/1974",
    gender: "f",
    hair_color: "brown",
    occupation: "manager",
    nationality: "irish"
});
coll.insert({
    first: "neil",
    last: "hanslem",
    dob: "14/07/1983",
    gender: "m",
    hair_color: "blonde",
    occupation: "actor",
    nationality: "british"
});
coll.insert({
    first: "rocky",
    last: "persolm",
    dob: "19/12/1994",
    gender: "f",
    hair_color: "black",
    occupation: "activist",
    nationality: "american"
});
Find all documents in collection:
coll.find();
Find all documents with gender == "f":
coll.find({gender: "f"});
Find all documents with gender == "f" AND nationality == "british":
coll.find({gender: "f", nationality: "british"});
Find all documents with gender == "f" AND nationality == "american" OR "irish":
coll.find({gender: "f", $or: [{nationality: "american"}, {nationality: "irish"}]});
Find all documents with gender == "f" AND nationality == "american" OR "irish", then sort by nationality (ascending):
coll.find({gender: "f", $or: [{nationality: "american"}, {nationality: "irish"}]}).sort({nationality: 1});
Find all documents with gender == "f" AND nationality == "american" OR "irish", then sort by nationality (descending):
coll.find({gender: "f", $or: [{nationality: "american"}, {nationality: "irish"}]}).sort({nationality: -1});


#mongo.py file

import os
import pymongo
if os.path.exists("env.py"):
    import env


MONGO_URI = os.environ.get("MONGO_URI")
DATABASE = "myFirstDB"
COLLECTION = "celebrities"


def mongo_connect(url):
    try:
        conn = pymongo.MongoClient(url)
        print("Mongo is connected")
        return conn
    except pymongo.errors.ConnectionFailure as e:
        print("Could not connect to MongoDB: %s") % e


conn = mongo_connect(MONGO_URI)

coll = conn[DATABASE][COLLECTION]

documents = coll.find()

""" Insert a single document """
# new_doc = {
#     "first": "douglas",
#     "last": "adams",
#     "dob": "11/03/1952",
#     "gender": "m",
#     "hair_color": "grey",
#     "occupation": "writer",
#     "nationality": "british"
# }
# coll.insert(new_doc)

""" Insert multipe documents """
# new_docs = [{
#     "first": "terry",
#     "last": "pratchett",
#     "dob": "28/04/1948",
#     "gender": "m",
#     "hair_color": "not much",
#     "occupation": "writer",
#     "nationality": "british"
# }, {
#     "first": "george",
#     "last": "rr martin",
#     "dob": "20/09/1948",
#     "gender": "m",
#     "hair_color": "white",
#     "occupation": "writer",
#     "nationality": "american"
# }]
# coll.insert_many(new_docs)

""" Find documents with 'first' name set to 'douglas' """
# documents = coll.find({"first": "douglas"})

""" Delete documents with 'first' name set to 'douglas' """
# coll.remove({"first": "douglas"})
# documents = coll.find()

""" Update a single document (first one only) """
# coll.update_one(
#     {"nationality": "american"},
#     {"$set": {"hair_color": "maroon"}}
# )
# documents = coll.find({"nationality": "american"})

""" Update all documents """
# coll.update_many(
#     {"nationality": "american"},
#     {"$set": {"hair_color": "maroon"}}
# )
# documents = coll.find({"nationality": "american"})

for doc in documents:
    print(doc)