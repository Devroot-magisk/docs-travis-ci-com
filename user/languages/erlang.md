---
title: Building a Erlang project
layout: en

---

### What This Guide Covers

<aside markdown="block" class="ataglance">

| Erlang            | Default                                       |
|:------------------|:----------------------------------------------|
| Typical `install` | `rebar get-deps`                              |
| Typical `script`  | `rebar compile && rebar skip_deps=true eunit` |
| Matrix keys       | `env`, `otp_release`                          |
| Support           | [Travis CI](mailto:support@travis-ci.com)     |

Minimal example:

```yaml
language: erlang
```
{: data-file=".travis.yml"}

</aside>

{{ site.data.snippets.linux_note }}

The rest of this guide covers build environment and configuration topics
specific to Erlang projects. Please make sure to read our
[OnBoarding](/user/onboarding/) and
[General Build configuration](/user/customizing-the-build/) guides first.

Erlang builds are not available on the macOS environment.

## Choosing OTP releases to test against

Travis CI VMs provide 64-bit [Erlang OTP](http://www.erlang.org/download.html) releases built using [kerl](https://github.com/spawngrid/kerl). To specify OTP releases you want your project to be tested against, use the `otp_release` key:

```yaml
language: erlang
otp_release:
  - "23.0.2"
  - "22.3.4"
```
{: data-file=".travis.yml"}

Get a complete list of the pre-compiled versions available on the VM by adding `kerl list installations` to the `before_script:` section of your `.travis.yml`. Note that this list does *not* include releases which are downloaded on demand, such as 18.1.

## Default Test Script

Travis CI by default assumes your project is built using [Rebar3](https://github.com/erlang/rebar3) and uses EUnit. The exact command Erlang builder will use by default is

```bash
rebar3 eunit
```

if your project has `rebar.config` or `Rebar.config` files in the repository root.

On older images where `rebar3` is not available, we fall back to [`rebar`](https://github.com/rebar/rebar), and call

```bash
rebar compile && rebar skip_deps=true eunit
```

If neither `rebar.config` nor `Rebar.config` is found in the repository root, Erlang builder will fall back to

```bash
make test
```

## Dependency Management

The Erlang builder on Travis CI assumes Rebar3 is used for dependency management.
See [Rebar3 documentation](http://www.rebar3.org/docs/dependencies) for further details.

On older images where `rebar3` is not available, we fall back to [`rebar`](https://github.com/rebar/rebar), and run

```bash
rebar3 get-deps
```

to install [project dependencies](https://github.com/basho/riak/blob/master/rebar.config) as listed in the `rebar.config` file.

## Environment Variable

The version of OTP release a job is using is available as:

```
TRAVIS_OTP_RELEASE
```

{% if site.data.language-details.erlang-versions.size > 0 %}

## Build Config Reference

You can find more information on the build config format for [Erlang](https://config.travis-ci.com/ref/language/erlang) in our [Travis CI Build Config Reference](https://config.travis-ci.com/).

## OTP/Release versions

These archives are available for on-demand installation.

{: #erlang-versions-table}
| Release | Arch | Name |
| :------------- | :------------- | :------- |{% for file in site.data.language-details.erlang-versions %}
| {{ file.release }} | {{ file.arch }} | {{ file.name }} |{% endfor %}
{% endif %}

## Examples

- [elixir](https://github.com/elixir-lang/elixir/blob/master/.travis.yml)
- [mochiweb](https://github.com/mochi/mochiweb/blob/master/.travis.yml)
- [ibrowse](https://github.com/cmullaparthi/ibrowse/blob/master/.travis.yml)

## Tutorial(s)

- [(English) Continuous Integration for Erlang With Travis-CI](http://blog.equanimity.nl/blog/2013/06/04/continuous-integration-for-erlang-with-travis-ci/)
- [(Dutch) Geautomatiseerd testen with Erlang en Travis-CI](http://blog.equanimity.nl/blog/2013/04/25/geautomatiseerd-testen-met-erlang/)

<script src="{{ "/assets/javascripts/tablefilter/dist/tablefilter/tablefilter.js" | prepend: site.baseurl }}" type="text/javascript" charset="utf-8"></script>
<script>
var tf = new TableFilter(document.querySelector('#erlang-versions-table'), {
    base_path: '/assets/javascripts/tablefilter/dist/tablefilter/',
    col_0: 'select',
    col_1: 'select',
    col_2: 'none',
    col_widths: ['100px', '100px', '250px'],
    alternate_rows: true,
    no_results_message: true
});
tf.init();
tf.setFilterValue(0, "16.04");
tf.setFilterValue(1, "x86_64");
tf.filter();
</script>
