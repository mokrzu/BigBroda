# GoogleBigquery

Big query client built on top of google api client.

https://developers.google.com/bigquery/what-is-bigquery


## Installation

Add this line to your application's Gemfile:

    gem 'google_bigquery'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install google_bigquery

## Usage

### Configuration setup:

  https://developers.google.com/bigquery/docs/authorization

  Configure GoogleBigquery client:

```ruby
GoogleBigquery::Config.setup do |config|
  config.pass_phrase = "notasecret"
  config.key_file    = /location/to_your/key_file.p12
  config.client_id   = "XXXXX.apps.googleusercontent.com"
  config.scope       = "https://www.googleapis.com/auth/bigquery"
  config.email       = "XXXXXX@developer.gserviceaccount.com"
end
```

  And authorize client:

```ruby
@auth = GoogleBigquery::Auth.new
@auth.authorize
```
  Then you are ready to go!


### Projects

  https://developers.google.com/bigquery/docs/reference/v2/projects

```ruby
GoogleBigquery::Project.list["projects"]
```

### Jobs

  https://developers.google.com/bigquery/docs/reference/v2/jobs

#### Query

```ruby
GoogleBigquery::Jobs.query(@project, {"query"=> "SELECT * FROM [#{@dataset_id}.#{@table_name}] LIMIT 1000" })
```


### Datasets

  https://developers.google.com/bigquery/docs/reference/v2/datasets

#### List:

```ruby
GoogleBigquery::Dataset.list(@project_id)
```

#### Create/Insert:

```ruby  
GoogleBigquery::Dataset.create(@project, {"datasetReference"=> { "datasetId" => @dataset_id }} )
```

#### Delete:

```ruby  
GoogleBigquery::Dataset.delete(@project, @dataset_id }} )
```

#### Update/Patch:

  Updates information in an existing dataset. The update method replaces the entire dataset resource, whereas the patch method only replaces fields that are provided in the submitted dataset resource.

```ruby  
GoogleBigquery::Dataset.update(@project, @dataset_id,
      {"datasetReference"=> {
       "datasetId" =>@dataset_id }, 
      "description"=> "foobar"} )
```


  Updates information in an existing dataset. The update method replaces the entire dataset resource, whereas the patch method only replaces fields that are provided in the submitted dataset resource. This method supports patch semantics.

```ruby
GoogleBigquery::Dataset.patch(@project, @dataset_id,
        {"datasetReference"=> {
         "datasetId" =>@dataset_id }, 
        "description"=> "foobar"} )
```


### Tables

  https://developers.google.com/bigquery/docs/reference/v2/tables

#### Create:

```ruby
@table_body = {  "tableReference"=> {
                    "projectId"=> @project,
                    "datasetId"=> @dataset_id,
                    "tableId"=> @table_name}, 
        "schema"=> [fields: 
                      {:name=> "name", :type=> "string", :mode => "REQUIRED"},
                      {:name=>  "age", :type=> "integer"},
                      {:name=> "weight", :type=> "float"},
                      {:name=> "is_magic", :type=> "boolean"}
                  ]
      }

GoogleBigquery::Table.create(@project, @dataset_id, @table_body
```   

#### Update:

```ruby
    GoogleBigquery::Table.update(@project, @dataset_id, @table_name,
        {"tableReference"=> {
         "projectId" => @project, "datasetId" =>@dataset_id, "tableId"  => @table_name }, 
        "description"=> "foobar"} )
```
       
#### Delete:

```ruby
GoogleBigquery::Table.delete(@project, @dataset_id, @table_name )
```

#### List:

```ruby
    GoogleBigquery::Table.list(@project, @dataset_id )
```

### Table Data
  
  https://developers.google.com/bigquery/docs/reference/v2/tabledata

#### InsertAll

Streaming data into BigQuery is free for an introductory period until January 1st, 2014. After that it will be billed at a flat rate of 1 cent per 10,000 rows inserted. The traditional jobs().insert() method will continue to be free. When choosing which import method to use, check for the one that best matches your use case. Keep using the jobs().insert() endpoint for bulk and big data loading. Switch to the new tabledata().insertAll() endpoint if your use case calls for a constantly and instantly updating stream of data.

```ruby
@rows =   {"rows"=> [
                      {
                        "insertId"=> Time.now.to_i.to_s,
                        "json"=> {
                          "name"=> "User #{Time.now.to_s}"
                        }
                      }
                    ]}

GoogleBigquery::TableData.create(@project, @name, @table_name , @rows )
```


#### List

```ruby
GoogleBigquery::TableData.list(@project, @dataset_id, @table_name)
```


## RESOURCES:

  https://developers.google.com/bigquery/loading-data-into-bigquery

  https://developers.google.com/bigquery/streaming-data-into-bigquery

### Api Explorer:

  https://developers.google.com/apis-explorer/#s/bigquery/v2/

### Google Big query developer guide

  https://developers.google.com/bigquery/docs/developers_guide

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
