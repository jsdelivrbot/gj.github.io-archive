---
layout: post
title:  "Project: Help Out NYC (jQuery)"
date:   2016-10-08 21:00:00 +0000
---

<h2>Project</h2>
<p>For my jQuery portfolio project, I refactored the views for the Issue model from my Rails project, Help Out NYC, using JavaScript and jQuery.</p>
<h2>Process</h2>
<p>The hardest part of the endeavor was in deciding which piece of the application to mess with. I spent a long time creating Help Out NYC, and I was mildly terrified of unknowingly tweaking some seemingly inconsequential thing and ending up with a completely broken app.</p>
<p>Those fears turned out to be completely unwarranted — JavaScript and jQuery are the best! To refactor most of the view features, I was able to simply make a one-to-one code replacement, removing ERB from the view templates and writing new code in the corresponding JS file. Before:
<pre class="prettyprint linenums">
&lt;%# app/views/issues/index.html %&gt;

&lt;tbody&gt;
  &lt;% @issues.each do |issue| %&gt;
    &lt;tr&gt;
      &lt;td&gt;&lt;%= link_to issue.name, issue %&gt;&lt;/td&gt;
      &lt;td class="table-center"&gt;&lt;%= issue.organizations.count %&gt;&lt;/td&gt;
      &lt;% if policy(issue).edit? %&gt;
        &lt;td&gt;&lt;%= link_to 'Edit', edit_issue_path(issue) %&gt;&lt;/td&gt;
      &lt;% end %&gt;
      &lt;% if policy(issue).destroy? %&gt;
        &lt;td&gt;&lt;%= link_to 'Destroy', issue, method: :delete, data: { confirm: 'Are you sure?' } %&gt;&lt;/td&gt;
      &lt;% end %&gt;
    &lt;/tr&gt;
  &lt;% end %&gt;
&lt;/tbody&gt;
</pre>

After:
<pre class="prettyprint linenums">
&lt;!-- app/views/issues/index.html --&gt;

&lt;tbody&gt;&lt;/tbody&gt;
</pre>

<pre class="prettyprint linenums">
// app/assets/javascripts/issues.js

function loadIssues() {
  $.getJSON('/issues', function(issues) {
    $.each(issues, function(index, i) {
      var issue = new Issue(i['id'], i['name'], i['organization_count'], i['organization_sample']);
      $('tbody').append(issue.formatForTable());
      $('#issue-' + issue.id).hover(function() {
        // populate #more-info div
        getMoreInfo(issue);
      }, function() {
        // clear the div when not hovering over anything
        $('#more-info').empty();
      });
      $('#issue-' + issue.id).on('click', function() {
        // redirect to show page
        window.location.replace('/issues/' + issue.id);
      });
    })
  })
}
</pre>
