---
layout: post
title:  "Project: Help Out NYC (Sinatra)"
date:   2016-08-10 12:00:00 +0000
---

<h2>Project</h2>
<p>For my Sinatra assessment project, I created Help Out NYC, a simple web app to help NYC residents advertise, search for, and sign up for local volunteer opportunities. The app allows users to:
<ul><li>sign up</li>
<li>log in and out</li>
<li>create new organizations and events</li>
<li>edit and delete organizations and events <em>that they created</em></li>
<li>view any particular organization, issue, event, or user</li>
<li>sign up for and resign from events that have not yet met their volunteer goal</li>
<li>delete their own user account</li></ul></p>

<h3>Setup</h3>
<p>To run the app on a local web server, clone <a href="https://github.com/gj/help-out-sinatra" target="_blank">the GitHub repository</a> down to your local machine, <code>cd</code> into the repo's local directory, run <code>bundle install</code>, run <code>rackup</code>, and then navigate to <a href="http://localhost:9292/" target="_blank">localhost:9292</a> in your web browser (preferably Chrome).</p>

<h3>Usage</h3>
<p>You will be unable to access any pages except <a href="http://localhost:9292/signup" target="_blank">/signup</a> and <a href="http://localhost:9292/login" target="_blank">/login</a> until you log in. Once logged in, you can:
<ul>
  <li>Create new:</li>
  <ul>
    <li><a href="http://localhost:9292/organizations/new" target="_blank">organizations</a></li>
    <li><a href="http://localhost:9292/events/new" target="_blank">events</a></li>
  </ul>
  <li>Edit existing:</li>
  <ul>
    <li><a href="http://localhost:9292/organizations/1/edit" target="_blank">organizations</a> <em>that you created</em></li>
    <li><a href="http://localhost:9292/events/1/edit" target="_blank">events</a> <em>that you created</em></li>
  </ul>
  <li>View lists of:</li>
  <ul>
    <li><a href="http://localhost:9292/organizations" target="_blank">organizations</a></li>
    <li><a href="http://localhost:9292/issues" target="_blank">issues</a></li>
    <li><a href="http://localhost:9292/events" target="_blank">events</a></li>
    <li><a href="http://localhost:9292/users" target="_blank">users</a></li>
  </ul>
  <li>View individual:</li>
  <ul>
    <li><a href="http://localhost:9292/organizations/1" target="_blank">organizations</a> <em>and delete those that you created</em></li>
    <li><a href="http://localhost:9292/issues/1" target="_blank">issues</a></li>
    <li><a href="http://localhost:9292/events/1" target="_blank">events</a> <em>and delete those that you created</em></li>
    <li><a href="http://localhost:9292/users/1" target="_blank">users</a></li>
  </ul>
  <li>Sign up for events that haven't yet met their volunteer goal.</li>
  <li>Remove yourself from the volunteer list for events for which you previously signed up.</li>
  <li>Log out.</li>
  <li>Delete your user account.</li>
</ul></p>

<h3>Dummy (and real!) data</h3>
<p>For testing purposes, the project comes with <a href="https://github.com/gj/help-out-sinatra/blob/master/db/seeds.rb" target="_blank">a seed file</a> that contains code to populate the app's database (<a href="https://github.com/gj/help-out-sinatra/blob/master/db/development.sqlite" target="_blank">db/development.sqlite</a>) with the following data:
<ul><li>970 real NYC organizations scraped from <a href="https://data.cityofnewyork.us/Social-Services/NYC-Women-s-Resource-Network-Database/pqg4-dm6b" target="_blank">the NYC Women's Resource Network Database</a></li>
<li>22 real issues scraped from the same database</li>
<li>5000 fake events generated randomly</li>
<li>200 fake users generated from <a href="https://www.ssa.gov/OACT/babynames/decades/century.html" target="_blank">the U.S. Social Security Administration's list of "the 100 most popular given names for male and female babies born during the last 100 years, 1916-2015"</a></li></ul></p>
