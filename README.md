# Discussion Questions: MVC and Rails Render

## Objectives

* Recall the MVC pattern from Rails
* Identify what `render` does in a Rails controller
* Implement controller actions that render different types

## MVC Review Questions

Write your best answers to the following questions, then discuss with your group.

1. What is the _Model_ responsible for in MVC?  What about the _Controller_? The _View_?

Model: the business logic of the app
Controller: communication with server, navication between pages
View: describing how pages should look

2. Now that we are using JavaScript to build Single Page Applications, what changes do we need to make to the division of responsibilities in MVC? What is JavaScript responsible for? What is Rails responsible for?

Add serializer to handle construction of json/api
Use rack-cors gem to deal with internal & external server calls

3. What kinds of formats can Rails render? List as many as you can. When you think you've got a full list, check out [Rails Guide on Rendering](https://guides.rubyonrails.org/layouts_and_rendering.html#using-render) to see if you missed any.

Rails can render erbs, plain text, json, xml...
Rails template, a file, inline, nothing, template...

4. Check out the other ways of sending data in Rails in the [ActionController::DataStreaming documentation](https://api.rubyonrails.org/v5.2.3/classes/ActionController/DataStreaming.html). Draw out how you would build a file download button.

# in routes:

get "home/download_pdf"

# in controller (already have pdf):

def download_pdf
  send_file(
    "#{Rails.root}/public/your_file.pdf",
    filename: "your_custom_file_name.pdf",
    type: "application/pdf"
  )
end

# in controller (need to generate pdf):

require "prawn"
class ClientsController < ApplicationController

  def download_pdf
    client = Client.find(params[:id])
    send_data generate_pdf(client),
              filename: "#{client.name}.pdf",
              type: "application/pdf"
  end

  # private

  def generate_pdf(client)
    Prawn::Document.new do
      text client.name, align: :center
      text "Address: #{client.address}"
      text "Email: #{client.email}"
    end.render
  end
end

# in view:

<%= link_to 'Download PDF', home_download_pdf_url %>

# Rails

def download
  send_data pdf,
    :filename => "abc.pdf",
    :type => "application/pdf"
end

## Exercise

Check out the rails app in the `rendering` folder. `bundle install` and `rails s` to get things running.

Open `config/routes.rb` and `app/controllers/responses_controller.rb`. Add the `render`, `send_data`, and `send_file` lines to the controller so that all the routes work. Check them all by visiting the urls in the browser.
