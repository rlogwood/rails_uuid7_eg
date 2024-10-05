rails g migration EnablePgcrypto

```ruby
class EnablePgcrypto < ActiveRecord::Migration[7.2]
  def change
    enable_extension 'pgcrypto'
  end
end

```


add to config/application.rb
```ruby
    config.generators do |generate|
      generate.orm :active_record, primary_key_type: :uuid
    end

```


app/model/application_record.rb
```ruby
require 'securerandom'

class ApplicationRecord < ActiveRecord::Base
  primary_abstract_class

  before_create :generate_uuid_v7

  private

    def generate_uuid_v7
      return if self.class.attribute_types['id'].type != :uuid

      #self.id ||= UUID7.generate
      self.id ||= SecureRandom.uuid_v7
    end  
end

```