# gmrender-resurrect

This role installs [gmrender-resurrect](https://github.com/hzeller/gmrender-resurrect) and creates a
[systemd](https://wiki.debian.org/systemd) service for it.
That service can be configured to your needs, using the role variables documented below.

## Requirements

An apt based system like [Ubuntu](https://www.ubuntu.com/) or [Debian](https://www.debian.org/).

## Updating

This role can also update gmrender-resurrect to a desired version.
To do so, just change the [source specification](#source-specification) and the
`gmrender_resurrect_version` variable accordingly, and then reapply this role.

## Role Variables

| Name                           | Default                                                                 | Description                                       |
| :----------------------------- | :---------------------------------------------------------------------- | :------------------------------------------------ |
| gmrender_resurrect_binary_url  | _undefined_                                                             | See [source specification](#source-specification) |
| gmrender_resurrect_archive_url | _undefined_                                                             | See [source specification](#source-specification) |
| gmrender_resurrect_github_repo | hzeller/gmrender-resurrect                                              | See [source specification](#source-specification) |
| gmrender_resurrect_checksum    | sha256:79dba7cbd275098026a16829597b7013ab71a9cca4c9762af6f210637099c39a | Checksum of the downloaded archive or binary      |
| gmrender_resurrect_version     | a7b0b1b9ca482d2d34ac62c2f2dc0cf0dfbb702b                                | See [source specification](#source-specification) |
| gmrender_resurrect_user        | gmrender-resurrect                                                      | The user that runs gmrender-resurrect.                     |

## Role Variables Configuring Flags and Options

The following role variables configure
[gmrender-resurrect flags or options, documented here](https://github.com/hzeller/gmrender-resurrect/blob/master/INSTALL.md#commandline-options).

| Name                           | Default       | Description                          |
| :----------------------------- | :------------ | :----------------------------------- |
| gmrender_resurrect_device_name | UPnP Renderer | Argument passed to `--friendly-name` |

### Source Specification

There are multiple ways to specify the source from which gmrender-resurrect should be downloaded.
Chosing among them is done by providing exactly one of the following role variables.
Actually, multiple of those role variables can be provided, but then only the one listed here first
is considered.

* `gmrender_resurrect_binary_url`:
  Download a precompiled binary from that URL.
  **You should anyway set `gmrender_resurrect_version` to a value that's unique for that version.**
  This is because gmrender-resurrect is only re-downloaded, when the `gmrender_resurrect_version`
  role variable changes.
* `gmrender_resurrect_archive_url`:
  Build from source.
  The source is downloaded as an archive from that URL.
  The downloaded archive must contain a folder named
  `gmrender-resurrect-{{ gmrender_resurrect_version }}` which contains in turn the source that can
  be compiled with `cargo build`.
  The compilation yields a binary named `gmrender-resurrect` which is then installed.
* `gmrender_resurrect_github_repo`: 
  Short form for
  `gmrender_resurrect_archive_url: https://github.com/{{ gmrender_resurrect_github_repo }}/archive/{{ gmrender_resurrect_version }}.tar.gz`.

## Example Playbook

The following playbook works if `~/.cache/stuvus` exists on the localhost.

```yml
- hosts: mpd01
  roles:
    - role: gmrender-resurrect
      global_cache_dir: "{{ lookup('env', 'HOME') }}/.cache/stuvus"

      gmrender_resurrect_device_name: stuvus-UPnP
      gmrender_resurrect_user: stuvus-mpd
```
