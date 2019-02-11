This repo is here to show the issue with buckets with same name but in different
cloud providers that are compatible with S3 including S3, and we can't access once
their region is cached.

- setup your `.env.template` and rename it to `.env`.
- create two buckets inside S3 and Wasabi with same name, but different region.`
- run: 
```
$ bundle install
$ rake main:list_content[<bucket_name>]
```

- This is happening because the buckets with cached region get an auto generated endpoint
that is hardcoded with `s3.amazonaws.com` instead of using the given endpoint when
the S3 client instance is created. This bit of code shows where this is happening:

```ruby
# aws-sdk-ruby/gems/aws-sdk-s3/lib/aws-sdk-s3/plugins/s3_signer.rb

# ...

def new_hostname(context, region)
  bucket = context.params[:bucket]
  if region == 'us-east-1'
    "#{bucket}.s3.amazonaws.com"
  else
    endpoint = Aws::Partitions::EndpointProvider.resolve(region, 's3')
    bucket + '.' + URI.parse(endpoint).host
  end
end

# ...
```
