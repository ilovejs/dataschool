---
layout: base
---
{% assign bookchapters = site.chapters | where:'book', page.book | sort: 'number' %}

{% assign book = site.books | where:'slug', page.book %}
{% assign book = book[0] %}

<div class="wrapper">
  <!-- Nav Sidebar -->
  <nav class="sidebar">
    <div class="sidebar-header">
      <a href="/"><img src="/uploads/dataschool-small.png"></a>
    </div>
    <ul class="list-unstyled components">
      <li><a href="/books/">Books</a></li>
      <li><a href="/mission/">Mission</a></li>
      <li><a href="/people/">Contributors</a></li>
      <li><a href="/books/">Slack Community</a></li>
    </ul>

    {% include smallbook.html %}

    <ul class="list-unstyled components">
      <li class="d-none">
        <ul>
          {% for chapter in bookchapters %}
            {% if currentsection != chapter.section %}
              {% assign currentsection = chapter.section %}
                </ul>
                </li>
                <li class="book-section ">
                  <a href="#{{currentsection}}" data-target="#{{currentsection}}" data-toggle="collapse" aria-expanded="true" class="dropdown-toggle">{{ currentsection }}</a>
                  <ul class="collapse list-unstyled {% if currentsection == page.section %}show{% endif %}" id="{{currentsection}}">
            {% endif %}
            <li>
              <a href="{{ chapter.url }}">
                {{ chapter.title }}
              </a>
            </li>
          {% endfor %}
        </ul>
      </li>
    </ul>
  </nav>
  <!-- End Nav Sidebar -->
  <!-- Page Content -->
  <div id="content">
    <button type="button" id="sidebar-collapse" class="btn btn-info">
      <i class="fas fa-align-left"></i>
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="justify-content-md-center">
      <div class="container page-content">
        <article>
          <header>
            <h1>{{ page.title | escape }}</h1>
            <p>last modified: {{ page.last_modified_at | date: '%B %d %Y' }}</p>
          </header>
          <section>
            {{ content }}
          </section>
          <p>
            <b>authors:</b>
            {% for author_slug in page.authors %}
              {% assign a = site.people | where: 'slug', author_slug %}
              {% assign author = a[0] %}
              <a href="{{ author.url }}">{{ author.title }}</a>{% if forloop.last != true %},{% endif %}
            {% endfor %}
          </p>
          <p>
            <b>reviewers:</b>
            {% for reviewer_slug in page.reviewers %}
              {% assign a = site.people | where: 'slug', reviewer_slug %}
              {% assign reviewer = a[0] %}
              <a href="{{ reviewer.url }}">{{ reviewer.title }}</a>{% if forloop.last != true %},{% endif %}
            {% endfor %}
          </p>
        </article>
      </div>
    </div>
  </div>
</div>

<!-- ------------------------------------------ -->
<!-- END OF PAGE, JAVASCRIPT FROM HERE OUT -->
<!-- ------------------------------------------ -->
<script type="text/javascript">
// Code for toggling the side menu
$(document).ready(function () {
  $('#sidebar-collapse').on('click', function () {
    $('#sidebar').toggleClass('active');
  });
});
</script>

{% if page.needs_sqlbox %}
  <!-- Needed for SQLBox - Man, there's just a lot needed here. -->
  <link rel="stylesheet" href="/assets/sqlbox/codemirror/codemirror.css">
  <link rel="stylesheet" href="/assets/sqlbox/codemirror/midnight.css">

  <script src="/assets/sqlbox/codemirror/codemirror.js"></script>
  <script src="/assets/sqlbox/codemirror/codemirror-mode-sql.js"></script>
  <script src="/assets/sqlbox/jquery.form.min.js"></script>

  <script type="text/javascript">
    // Data needed for sqlbox.js to work
    $.sqlboxSettings = {
      dbType: '{{ page.database }}',
      dbName: '{{ page.dbname }}',
      action: '{{ site.sqlboxurl }}'
    };
  </script>
  <script type="text/javascript" src="/assets/sqlbox/sqlbox.js"></script>
  <link rel="stylesheet" href="https://unpkg.com/bootstrap-table@1.14.2/dist/bootstrap-table.min.css">
  <script src="https://unpkg.com/bootstrap-table@1.14.2/dist/bootstrap-table.min.js"></script>
  <!-- End for SQLBox -->
{% endif %}