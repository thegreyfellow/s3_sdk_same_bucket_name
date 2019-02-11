- setup your `.env.template` and rename it to `.env`.
- create two buckets inside S3 and Wasabi with same name, but different region.`
- run: 
```
$ bundle install
$ rake main:list_content[<bucket_name>]
```

