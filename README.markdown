# Yardoc RESTful Web Service Plugin

originally by [vWorkApp](http://www.vworkapp.com)
rewritten for 0.3.0 by [rknLA](http://github.com/rknLA) with substantial help from [lsegal](http://gnuu.org/)

A plugin for [Yardoc](http://yardoc.org/) that generates documentation for RESTful web services.

## Install original gem
    sudo gem install yard-rest

It also requires the Jeweler gem if you plan to use the rake build tasks.

## Generating Docs

When using yardoc you ask it to use the "rest" plugin (the --plugin option). For example: 

    yardoc --plugin rest --title "Our App's API"

## Gemfile functionality

You may also include yard-rest in your gemfile using:

    gem 'yard-rest'

You may need to include the following dependencies as well:

    gem 'redcarpet'
    gem 'yard', '~>0.7.4'

If you include yard-rest in your gemfile, you should generate docs using bundle exec:

    bundle exec yardoc --plugin rest --title "Our App's API"

## Using this fork

Above instructions are for the main gem.
This fork has not been released to a gem repository yet. If it ripens I might do so.

For now, to use it you may want to generate the gem locally or write in your gemfile:

    gem 'yard-rest', git: 'git://github.com/dwaynemac/yard-rest-plugin.git'

## Writing Comments

In addition to starting your comment with the normal RDoc description. The following tags are provided:

- @url url. Specifies the URL that the service is accessed from. This tag is compulsory, only **classes** and **methods** that include this in their comments are included.

- @topic topic. Specifies the topic to categorise a **class** (not a method) under.

- @argument [type] name description. Specifies an argument that must be passed to the service. You can specify as 
    many of these as you need.

- @optional_argument [type] name description. Specifies an optional argument that may be passed to the service. You can specify as 
    many of these as you need. 

- @key_for hash_name [type] name description. Specifies element of a hash argument.

- @value_for argument_name value description. Specifies valid value of an argument.

- @example_request example. An example of the request that is send to the service.

- @request_field name description. Further specifies the fields that are send within the request.  This is useful when the service 
    accepts only one argument that has many fields, like a JSON or XML string.

- @example_response example. An example of the response that is returned from the service

- @response_field name description. Further specifies the fields that are returned within the response

- @response_code code

- @header

- @image

## Ignored Documentation

This plugin only documents **classes** and **methods** with **@url** tags. It does not support module documentation.

The rationale here is that you are documenting external services (as represented by controllers and methods), and not internal code.

Both controller *and* methods must have @url and @topic tags to be included in documentation.

Methods with the @overall tag will be ignored.

## Example:

    ##
    # Retuns all samples, as XML, for the current user that match the given parameters.
    #
    # @url [GET] /samples.[format]?[arguments]
    # @url [GET] /samples/index.[format]?[arguments]
    # 
    # @required_argument [String] format Only "xml" is support at this time.
    # @required_argument [String] name The name of the sample
    # @required_argument [String] resource The resource that sample belongs to
    # @optional_argument ["@assigned"|"@complete"|"!@complete"] search Return samples that are assigned, complete, or
    #   uncomplete.
    #
    # @example_response
    #   <samples type="array">
    #     <sample>
    #       <id>961</id>
    #       <name>My Sample</name>
    #       <state>complete</state>
    #       <last_unassigned_user_id type="integer"></last_unassigned_user_id>
    #       <resource_id type="integer">127</resource_id>
    #       <notes></notes>
    #       <updated_at type="datetime">2010-03-09T20:43:29Z</updated_at>
    #       <created_at type="datetime">2010-03-09T20:43:16Z</created_at>
    #     </sample>
    #   <samples>
    # 
    # @response_field [Integer] id A unique ID identifying the Sample
    # @response_field [String] name The name of the sample
    # @response_field [String] state The current status of the Sample. Can be complete, uncomplete, etc.
    # @response_field [String] notes Any notes given for the sample
    # @response_field [DateTime] updated_at The Date/Time (in ISO8601) that the Sample was last updated
    # @response_field [DateTime] created_at The Date/Time (in ISO8601) that the Sample was created
    # 
    def index
    end
    
    ##
    # Retuns all samples, as XML, for the current user that match the given parameters.
    # 
    # @url [POST] /samples.[format]?[arguments]
    # 
    # @required_argument [String] format Only "xml" is support at this time.
    #
    # @example_request
    #   <sample>
    #     <id>961</id>
    #     <name>My Sample</name>
    #     <state>complete</state>
    #     <last_unassigned_user_id type="integer"></last_unassigned_user_id>
    #     <resource_id type="integer">127</resource_id>
    #     <note_attributes type="array">
    #       <note>
    #         <id>new_123</id>
    #         <text>Note One</note>
    #       </note>
    #     </note_attributes>
    #     <updated_at type="datetime">2010-03-09T20:43:29Z</updated_at>
    #     <created_at type="datetime">2010-03-09T20:43:16Z</created_at>
    #   </sample>
    #
    # @request_field [Integer] id A unique ID identifying the Sample
    # @request_field [String] name The name of the sample
    # @request_field [String] state The current status of the Sample. Can be complete, uncomplete, etc.
    # @request_field [String] note_attributes Any notes given for the sample that will be created
    # @request_field [DateTime] updated_at The Date/Time (in ISO8601) that the Sample was last updated
    # @request_field [DateTime] created_at The Date/Time (in ISO8601) that the Sample was created
    #
    # @example_response
    #   <sample>
    #     <id>961</id>
    #     <name>My Sample</name>
    #     <state>complete</state>
    #     <last_unassigned_user_id type="integer"></last_unassigned_user_id>
    #     <resource_id type="integer">127</resource_id>
    #     <notes type="array">
    #       <note>
    #         <text>Note One</note>
    #       </note>
    #     </notes>
    #     <updated_at type="datetime">2010-03-09T20:43:29Z</updated_at>
    #     <created_at type="datetime">2010-03-09T20:43:16Z</created_at>
    #   </sample>
    # 
    # @response_field [Integer] id A unique ID identifying the Sample
    # @response_field [String] name The name of the sample
    # @response_field [String] state The current status of the Sample. Can be complete, uncomplete, etc.
    # @response_field [String] notes Any notes given for the sample
    # @response_field [DateTime] updated_at The Date/Time (in ISO8601) that the Sample was last updated
    # @response_field [DateTime] created_at The Date/Time (in ISO8601) that the Sample was created
    #
    def create
    end

## Development

As always, you can see what tasks are available by running:
    
    rake -T

You can run the template locally over the included sample code by using the following rake tasks:
    
    rake ex:clean
    rake ex:generate



