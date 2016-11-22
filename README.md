# Fluent::Plugin::Logentries
Forward logs to Logentries, using token based input.

Looks at the tag/message to find out where the log should go.

## Installation

install with gem or fluent-gem command as:

### native gem
    $ gem install fluent-plugin-logentries

### fluentd gem
    $ /opt/td-agent/embedded/bin/fluent-gem install fluent-plugin-logentries

## Configruation file (YML)

```yaml
    My-Awesome-App:
       app: MY-LOGENTRIES-TOKEN
       stdout: ANOTHER-LOGENTRIES-TOKEN (*)
       stderr: ANOTHER-LOGENTRIES-TOKEN-1 (*)
    Another-app:
       app: 2bfbea1e-10c3-4419-bdad-7e6435882e1f
       access: 5deab21c-04b1-9122-abdc-09adb2eda22 (*)
       error: 9acfbeba-c92c-1229-ccac-12c58d82ecc (*)
```
(\*) any token other than `app` is optional, if you don't use multiple log per host just provide an app token.

This file is read on changes, it allows on fly modifications.

## Usage

```
    <match pattern>
      type logentries
      config_path /path/to/logentries-tokens.conf
    </match>
```

## Parameters

### type (required)
The value must be `logentries`.

### config\_path (required)
Path of your configuration file, e.g. `/opt/logentries/tokens.conf`

### protocol
The default is `tcp`.

### use\_ssl
Enable/disable SSL for data transfers between Fluentd and Logentries. The default is `true`.

### port
Only in case you don't use SSL, the value must be `80`, `514`, or `10000`. The default is `20000` (SSL)

### max\_retries
Number of retries on failure.

### use\_json
When set to true, emit the entire record as a stringized JSON body instead of extracting the text from the `message` field of the record.

### app\_name
Used as a key for picking up a token set in the configuration file.  You can use placeholders that are compatible with fluent-plugin-record-modifier.

The default value is `${record['app_name']}`.


### token\_name
Used as a key for picking up a token in the token set determined by `app_name`.  You can use placeholders that are compatible with fluent-plugin-record-modifier.

The default value is `${tag == @tag_access_log ? 'access': tag == @tag_error_log ? 'error': 'app'}`


### tag\_access\_log, tag\_error\_log **(deprecated)**
This is use in case you tag your access/error log and want them to be push into another log.

You can now use an arbitrary key to specify the token by giving an express to `token_name` setting.


## Contributing

1. Fork it ( http://github.com/woorank/fluent-plugin-logentries/fork )
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

## MIT
THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
