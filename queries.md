1. show exactly two documents from the `listings` collection in any order
  ```Javascript
  db.listings.find().limit(2)
  ```
2. show all of the listings hosted by either "Kamilla" or "Sonder"
  + only show the name, price, neighbourhood, and host_name

  ```Javascript
  db.listings.find({$or: [{'host_name': 'Kamilla'}, {'host_name': 'Sonder'}]}, {'name':1,'price':1,'neighborhood':1})
  ```
3. find all the unique `host_name`
  ```Javascript
  db.listings.distinct('host_name')
  ```
4. find all of the places that have more than 2 `beds` in `city` Brooklyn, ordered by `review_scores_rating` descending
  + only show the `name`, `beds`, `city`, `review_scores_rating`, and `price`

  ```Javascript
  db.listings.find({'beds': {$gt: 2}, 'city': 'Brooklyn', 'review_scores_rating': {$ne: ''}}, {'name':1, 'price':1, 'neighborhood': 1}).sort({'rating': -1})
  ```
5. show the number of listings per host
  ```Javascript
  db.listings.find().aggregate([{$group: {'_id': '$host_name', num_listings: {$sum: 1}}}])
  ```
6. in city, New York, find the average `review_scores_rating` per `neighbourhood`, and only show the ones above a 95… sorted in descending order of rating
  ```Javascript
  db.listings.find().aggregate([{$group: {'_id': '$neighborhood', avg_review: {$avg: '$review_scores_rating'}}}, {$match: {'avg_review': {$gt: 95}}}]).sort({'rating': -1})
  ```
