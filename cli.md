---
title: Command Line Tool
layout: default
---

<style>
  code {
    background-color: #ddd;
    white-space: nowrap;
    display: inline-block;
    margin: 0 3px;
    padding: 1px 4px;
    border-radius: 5px;
  }
</style>

<ul>
<li><a href="#overview">Overview</a></li>
<li><a href="#import">Import</a>
  <ul>
    <li><a href="#clear-metadata">Clear Metadata</a></li>
  </ul>
</li>
<li><a href="#test-env">Environment Test</a></li>
<li><a href="#migrations">Migrations</a>
  <ul>
    <li><a href="#migrations-migrate">Migrate the Database</a></li>
    <li><a href="#migrations-writing">Writing Migrations</a></li>
  </ul>
</li>
<li><a href="#editions">Editions</a>
  <ul>
    <li><a href="#editions-list">List Editions</a></li>
    <li><a href="#editions-create">Create Editions</a></li>
    <li><a href="#editions-delete">Delete Editions</a></li>
  </ul>
</li>
</ul>

<h2><a id="overview"></a>Overview</h2>

<p>State Decoded comes with a command line tool, <code>statedecoded</code>. This
tool provides a non-web method to perform common actions, such as importing data
and managing editions.</p>

<p><strong>Note:</strong> to use this tool, the PHP CLI will need to be
installed.  (This is installed by default on most systems that have PHP.) On
linux/unix systems, the tool should be set to be executable to be able to run it
directly, but it may also be run as a normal PHP script (<code>php statedecoded
<em>arguments</em></code>); the examples below assume it is being run as a
standalone executable. Due to path resolution, you'll probably need to run it
with the path prefix as well – if you're in the root of the statedecoded
directory, it will be <code>./statedecoded</code>.</p>

<p>All commands are run with the <code>statedecoded</code> command.  You can run
<code>statedecoded help</code> to see all available commands, and you can use
<code>statedecoded help <em>command</em></code> to see an explanation of a
specific command along with all available options for it.</p>

<p>Many commands use the <code>-v=<em>#</em></code> option to change the output
verbosity. Messages are weighted by importance, so a verbosity of 1 will show
all message, up to a maximum of 10 for only important messages. Any number above
10 should silence most, if not all messages.</p>

<h2><a id="import"></a>Import</h2>

<p>Instead of having to use the admin area of The State Decoded to parse data
files to import legal codes, the command line tool can be run.  This is helpful
if the data is provided on a regular basis, allowing the script to be run as
part of a <code>cron</code> job, scheduled task, or similar.</p>

<p>The default import command (<code>statedecoded import</code>) will import
data from the files in the directory specified in the config file as
<code>IMPORT_DATA_DIR</code>. You may override this to use a different directory
with the <code>-d=<em>directory</em></code> option.</p>

<p>By default, the data is imported into the current edition. You may specify a
different edition to update with the <code>--edition=<em>slug</em></code> option
by specifying the edition slug.  Note that you cannot create a new edition using
the <code>import</code> command, you must use the
<code><a href="#editions-create">Create Edition</a></code> command first.</p>

<p>You may make the selected edition current by using the <code>--current</code>
option.</p>

<h3><a id="clear-metadata"></a>Clear Metadata</h3>

<p>You may clear the metadata associated with laws via the
<code>statedecoded clear-metadata</code> command. By default this removes all
metadata associated with laws for all editions. You may specify a specific
edition to clear with the <code>--edition=<em>slug</em></code> option, and/or
a particular field to clear out with the <code>--field=<em>field</em></code>
option.  For instance, to clear out all court decisions (which is a good thing
to do every few months to remove stale data), you may run the command
<code>statedecoded --field=court_decisions</code></p>

<h2><a id="test-env"></a>Environment Test</h2>

<p>You can test whether your environment is setup properly to run The State
Decoded by running the <code>statedecoded test-env</code> command. This test
is also run automatically before attempting to import data. If the environment
is not sufficient, errors will be listed, and the command will return with an
error code of 1.</p>

<h2><a id="migrations"></a>Migrations</h2>

<p>When writing new code for The State Decoded, you may need to modify the
database - to create tables, modify existing ones, or manipulate data.  To do
this in a way that is repeatable without modifying our base database
definitions, we use <em>database migrations</em>.</p>  These are scripts that
describe the changes to the database in a way that can easily be reproduced.

<h3><a id="migrations-migrate"></a>Migrate the Database</h3>

<p>To run all migrations that have not been run yet, use the <code>statedecoded
migrate</code> command. To "roll back" (undo) all of the migrations, use the
<code>statedecoded migrate --down</code> option.

<h3><a id="migrations-writing"></a>Writing Migrations</h3>

<p>To generate a new database migration file, run the <code>statedecoded
generate-migration</code> command.  This command will print out the name of the
newly-generated migration file.</p>

<p>Inside of this file, you will find a class with two methods: an
<code>up</code> method for making changes to the database, and a
<code>down</code> method for undoing those changes when rolling back.  You
should fill out both methods when creating a migration, if all changes are able
to be undone. In general, MySQL will not let you modify a database table such
that data is truncated, such as when chaning the size of a field or index – so
for those operations you should <em>not</em> provide a <code>down</code>
method.</p>

<p>Migrations use standard SQL queries, wrapped by our database wrapper into a
single database transaction.  The <code>$this->queue()</code> method is used
to queue up queries, and these must be run by the <code>$this->execute()</code>
method. Please have a look at the existing migration files in the
<code>includes/migrations/</code> directory for examples.</p>

<p><strong>Note:</strong>Migrations are run in the order of their file names.
These files are timestamped to avoid collisions. Unlike some other migration
tools, we keep track of which migrations have been run and execute all
migrations that have not been run, so you shouldn't need to worry out-of-order
migrations being lost from merging branches.</p>


<h2><a id="editions"></a>Editions</h2>

<p>You can manipulate editions with several builtin commands.</p>

<p><strong>Note:</strong> As of now, you <em>cannot</em> make an edition
current with these commands, you must do that at the time you are importing
data. This is due to the complex changes to permalinks that must occur when an
edition is made current.</p>

<h3><a id="editions-list"></a>List Editions</h3>

<p>To get the name of the current edition, you can use the <code>statedecoded
edition</code> command. To get a list of all editions, use the
<code>statedecoded edition list</code> command. For both of these commands, you
may add the <code>-v</code> (verbose) flag to receive all of the data about the
editions as a JSON-formatted string.</p>

<h3><a id="editions-create"></a>Create Editions</h3>

<p>To create a new edition, use the <code>statedecoded edition create
--name=<em>name</em></code> command.  You may optionally specify a slug for the
edition with the <code>--slug=<em>slug</em></code> flag, otherwise the slug will
be generated based on the name provided.  Note that if your name has spaces in
it, you <em>must</em> provide the name in quotes, e.g. <code>statedecoded
edition create --name="My New Edition"</code>.

<h3><a id="editions-delete"></a>Delete Editions</h3>

<p>You may delete an edition by its slug with the <code>statedecoded edition
delete --slug=<em>slug</em></code> command.</p>
