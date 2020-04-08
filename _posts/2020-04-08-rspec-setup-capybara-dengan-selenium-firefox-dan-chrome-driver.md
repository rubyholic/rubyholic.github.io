---
layout: post
title: Rspec - Setup Capybara dengan Selenium Firefox dan Chrome Driver
date: 2020-04-08 17:09 +0700
categories: [rspec, rails, capybara, selenium]
---

Asumsi saya kalian sudah paham dengan Rspec dan Capybara. Sedikit introduksi saja, capybara adalah suatu tools yang digunakan untuk membuat acceptence test atau feature test jika di rspec.

Berikut ini cara mengintegrasikan capybara dengan selenium dengan driver Firefox dan Google Chrome.

## Langsung Praktek

1. Tambahkan gem `capybara`, `rspec-rails` dan `webdrivers` pada Gemfile di project rails kalian:

    ```ruby
    ..

    group :test do
	gem 'capybara'
	gem 'rspec-rails'
	gem 'webdrivers'
	...
    ```

2. Kemudian unduh `chromedriver` melalui tautan berikut:
    <https://chromedriver.chromium.org/downloads>

3. Sunting berkas `rails_helper.rb` lalu tambahkan skrip di bawah ini:

    ```ruby
    require 'capybara/rspec'

    RSpec.configure do |config|
        Capybara.server = :puma, { Silent: true }
        Capybara.run_server = true

        Capybara.register_driver :headless_chrome do |app|
    	caps = Selenium::WebDriver::Remote::Capabilities.chrome(loggingPrefs: { browser: 'ALL' })
    	opts = Selenium::WebDriver::Chrome::Options.new

    	chrome_args = %w[--headless --no-sandbox --disable-gpu --window-size=1920,1080 --remote-debugging-port=9222]
    	chrome_args.each { |arg| opts.add_argument(arg) }
    	Capybara::Selenium::Driver.new(app, browser: :chrome, options: opts, desired_capabilities: caps)
        end

        Capybara.register_driver :chrome do |app|
    	Capybara::Selenium::Driver.new app, :browser => :chrome
        end

        Capybara.register_driver :firefox do |app|
    	profile = Selenium::WebDriver::Firefox::Profile.new
    	profile['plugin.state.flash'] = 0
    	profile['browser.download.dir'] = DownloadHelper::DOWNLOAD_PATH.to_s
    	profile['browser.download.folderList'] = 2
    	profile['browser.helperApps.neverAsk.saveToDisk'] = 'text/csv'
    	firefox_options = { firefox_profile: profile }

    	if ENV['SELENIUM_FIREFOX_PATH']
    	    firefox_options[:firefox_binary] = ENV['SELENIUM_FIREFOX_PATH']
    	end

    	capabilities = Selenium::WebDriver::Remote::Capabilities.firefox(firefox_options)
    	Capybara::Selenium::Driver.new(app, browser: :remote, url: "http://#{SELENIUM_SERVER}:4444/wd/hub", desired_capabilities: capabilities)
        end

        Capybara.default_driver = :headless_chrome
        Capybara.javascript_driver = :headless_chrome
    end
    ```

Selesai sekarang kalian bisa jalankan feature test kalian di rspec, dengan selenium. Skrip di atas, default drivernya adalah `headless_chrome`. Kalau mau pakai Firefox atau Google Chrome browser bisa langsung kalian ganti saja menjadi `:firefox` atau `:chrome`.

## Tambahan

Sebagai tambahan kalau kalian ingin buat selenium browser untuk Opera Mini, kalian bisa lakukan dengan mengganti user-agentnya seperti berikut:

```ruby
Capybara.register_driver :opera_mini do |app|
  profile = Selenium::WebDriver::Firefox::Profile.new
  profile['general.useragent.override'] = 'Opera/9.80 (Android; Opera Mini/7.6.35766/35.5706; U; en) Presto/2.8.119 Version/11.10'
  firefox_options = { firefox_profile: profile }
  if ENV['SELENIUM_FIREFOX_PATH']
    firefox_options[:firefox_binary] = ENV['SELENIUM_FIREFOX_PATH']
  end
  capabilities = Selenium::WebDriver::Remote::Capabilities.firefox(firefox_options)
  Capybara::Selenium::Driver.new(app, browser: :remote, url: "http://#{SELENIUM_SERVER}:4444/wd/hub", desired_capabilities: capabilities)
end
```

Semoga bermanfaat :)
