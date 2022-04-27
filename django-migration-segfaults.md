Title: Django migration segfaults to be resolved in 1.9
Slug: django-migration-segfaults
Date: 2015-11-13 00:03:48
Tags: python, django
Summary: How Django 1.9 resolves the rendering model states segfault.
Status: Published
Schema: Article
Keywords: django, model, state, segmentation, fault, migration

Upgrading your projects' dependencies requires too much effort, it has to be
handled carefully and more often than not, tends to drastically fail. We've not-so
recently decided to migrate one of our projects out of Django `1.6.X` onto `1.8.5`
and despite the amazing pluses we get from django 1.8, we were hit with big slap
in the face with django migrations.

Despite the extremely slow process of creating the database and applying all the
migrations, shortly after firing the `migrate` command during the `Rendering model states`
we were greeted with:

    Segmentation fault (core dumped)

It was simply impossible to debug this issue within our project because we deal
with not dozens, but hundreds of models (tables) as well as heavy relationships
between these models. What we needed to do is try and duplicate
this issue outside the scope of the project and in an isolated environment with
bare minimum dependencies. Furthermore, the models should be random in order
to properly test against different schema sizes.

Given the above requirements, I built a django random app generator that pours out
a massive number of apps, models and fields in order to properly test the migrations
on a project marginally larger from a database schema perspective than others.

The test-app-builder can be found [here](https://github.com/omaraboumrad/test-app-builder)
 alongside instructions on how to run it. The target project the upcoming tests
were running against can be found [here](https://github.com/omaraboumrad/tester).

### The Setup

Here's the setup for the tests being run.

    :::text
    % vagrant --version
    Vagrant 1.7.4
    % VBoxManage --version
    5.0.4r102546
    % grep 'box =' Vagrantfile
    config.vm.box = "ubuntu/trusty64"

The Vagrantfile is the absolute default built by the vagrant tool.

### Test #1 - Against Django 1.8.6

<script type="text/javascript" src="https://asciinema.org/a/00oyqsi8ezh6ile35s1n954rt.js" id="asciicast-00oyqsi8ezh6ile35s1n954rt" async></script>

It turned out that the issue was pre computing the model states consumed way too
much memory. The segmentation fault error could be solved in two different ways:

- Increase the memory of the machine you're running migrations in. In this specific
instance, Going upwards to 2GB would've fixed it.
- Migrate each app by itself, which is complex since you have to deal with the migration
dependencies manually but in this specific instance there's no dependencies so each
app could be migrated by itself.

### Test #2 - Against Django 1.9 Beta1

<script type="text/javascript" src="https://asciinema.org/a/b8w9p2xcojhew8o0vehmzlor6.js" id="asciicast-b8w9p2xcojhew8o0vehmzlor6" async></script>

Thankfully, Django 1.9 Beta1 has resolved this issue and life is good again! The
relevant [section from the 1.9 release notes](https://docs.djangoproject.com/en/1.9/releases/1.9/#migrations) states the following concerning this matter:

_"When applying migrations, the 'Rendering model states' step that's displayed
when running migrate with verbosity 2 or higher now computes only the states for
the migrations that have already been applied. The model states for migrations
being applied are generated on demand, drastically reducing the amount of
required memory."_

This improvement also comes with its own requirement, though this is a major relief!

Thank you `Django` for fixing this. <3

Enjoy
