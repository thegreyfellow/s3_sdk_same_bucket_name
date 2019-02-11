require 'dotenv'
require 'aws-sdk-s3'
# require 'pry'

namespace :main do
  desc 'List content of two buckets with the same name but in different\
  cloud providers.'
  task :list_content, [:bucket_name] do |_t, args|
    # load env variables
    Dotenv.load('.env')

    bucket_name = args[:bucket_name]
    # binding.pry
    s3_client = Aws::S3::Client.new(
      region: 'us-east-1',
      credentials: Aws::Credentials.new(
        ENV['s3_access_key_id'],
        ENV['s3_secret_access_key']
      )
    )

    # for debugging purposes I used http, this way using any packet tracer,
    # we can easily see which endpoint is used wasabi or s3
    wasabi_client = Aws::S3::Client.new(
      region: 'us-east-1',
      endpoint: 'http://s3.wasabisys.com',
      credentials: Aws::Credentials.new(
        ENV['wasabi_access_key_id'],
        ENV['wasabi_secret_access_key']
      )
    )

    puts '#list_objects_v2 results using: s3'
    s3_client.list_objects_v2(bucket: bucket_name, max_keys: 10)

    begin
      puts '#list_objects_v2 results using: wasabi'
      wasabi_client.list_objects_v2(bucket: bucket_name, max_keys: 10)
    rescue Aws::S3::Errors::InvalidAccessKeyId => e
      puts '*' * 20
      puts e.message
      puts '*' * 20
    end
  end
end
