# API Reference

Welcome to the API documentation. It's intended as a feature reference to give you more detailed information about working with F3.
You can also find extensive working examples in the [source code of the framework unit tests](https://github.com/bcosca/fatfree/tree/dev/app).

<div class="row-fluid pb25 ref">
    <div class="span6">
        <h3>Framework Core</h3>
        <a class="label" href="base" title="The Base class represents the framework core">Base</a>
        <a class="label" href="cache" title="F3 Multi protocols Cache engine">Cache</a>
        <a class="label" href="iso" title="The ISO class provides a list of ISO codes of languages  and countries">ISO</a>
        <a class="label" href="prefab-registry" title="Prefab is a factory wrapper for singleton classes">Prefab & Registry</a>
        <a class="label" href="view" title="The View is responsible for rendering PHP views in MVC parlance">View</a>
        <a class="label" href="preview" title="The Preview class is a lightweight template engine class that extends the View class">Preview</a>

        <h3>Databases</h3>
        <a class="label" href="jig" title="The Jig class provides a simple way to store arrays of data into flat ASCII files">Jig</a>
        <a class="label" href="mongo" title="The Mongo class is a lightweight MongoDB wrapper">Mongo</a>
        <a class="label" href="sql" title="The SQL class provides a lightweight and consistent interface for accessing SQL databases">SQL</a>

        <h3>Data Mappers</h3>
        <a class="label" href="cursor" title="This Cursor class is an abstract foundation of an Active Record implementation, that is used by all F3 Data Mappers">Cursor</a>
        <a class="label" href="basket" title="Session-based pseudo-mapper">Basket</a>
        <a class="label" href="jig-mapper" title="The Jig Object-Document-Mapper is an implementation of the abstract Active Record Cursor class">Jig Mapper</a>
        <a class="label" href="mongo-mapper" title="The Mongo Object-Document-Mapper (ODM) is an implementation of the abstract Active Record Cursor class">Mongo Mapper</a>
        <a class="label" href="sql-mapper" title="The SQL Object-Relational-Mapper is an implementation of the abstract Active Record Cursor class">SQL Mapper</a>
    </div>
    <div class="span6">
        <h3>Templating</h3>
        <a class="label" href="markdown" title="This is F3's own implementation of Markdown">Markdown</a>
        <a class="label" href="template" title="F3's own lightning fast and extendable template engine gives you all the flexibility you need for modern and clean templating">Template</a>

        <h3>Web Services</h3>
        <a class="label" href="geo" title="The Geo plugin gives you a handful of location-based features">Geo</a>
        <a class="label" href="openid" title="The OpenID class is a OpenID consumer">OpenID</a>
        <a class="label" href="pingback" title="Pingback 1.0 protocol (client and server) implementation">PingBack</a>
        <a class="label" href="smtp" title="The SMTP class is a SMTP plug-in">SMTP</a>
        <a class="label" href="web" title="The Web class and its descendants Geo and Pingback, contain several helpers to interact with HTTP clients and servers">Web</a>

        <h3>Misc</h3>
        <a class="label" href="audit" title="The Audit class is a data validator">Audit</a>
        <a class="label" href="auth" title="The Auth class is used to authenticate against several storages">Auth</a>
        <a class="label" href="bcrypt" title="The Bcrypt plugin is a secure and lightweight password hashing library">Bcrypt</a>
        <a class="label" href="image" title="The Image class offers a bunch of image processing features">Image</a>
        <a class="label" href="log" title="This is the F3 custom Logger class">Log</a>
        <a class="label" href="magic" title="A PHP magic wrapper">Magic</a>
        <a class="label" href="matrix" title="The matrix class offers some generic array utilities">Matrix</a>
        <a class="label" href="session" title="The SESSION handlers of the framework">Session</a>
        <a class="label" href="test" title="The Test class is a simple unit testing stack, where you can add multiple tests to and serve out their results">Test</a>
        <a class="label" href="utf-unicode-string-manager" title="The UTF class is an utility class that eases the handling of Unicode strings">UTF</a>

    </div>
</div>


The framework API documentation is also contained in the distribution package, at `lib/api/index.html` . F3 uses [Doxygen](http://www.stack.nl/~dimitri/doxygen/ "Doxygen is a tool for generating documentation from annotated source code") to create basic reference documentation in HTML format from comments in the F3 source files.
