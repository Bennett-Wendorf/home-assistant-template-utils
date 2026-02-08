# Home Assistant Template Utils

A collection of Jinja macros and utilities for Home Assistant templates. These templates extend the functionality of Home Assistant's templating system with useful utilities for parsing natural language, working with dates and times, and more.

## 📚 Available Templates

### Natural Language Parsing (`natural_language_parsing.jinja`)

Parse natural language expressions for numbers, dates, times, and durations in your Home Assistant automations and templates.

**Features:**
- 🔢 Convert text numbers to integers ("twenty-five" → 25)
- ⏰ Parse relative time expressions ("in 5 minutes" → 5 minutes)
- 📅 Parse relative dates ("tomorrow" → tomorrow's date)
- ⏱️ Convert durations to seconds ("2 hours" → 7200)
- 🕐 Parse time of day expressions ("3pm" → "15:00")

## 🚀 Installation

### Method 1: Using custom_templates directory (Recommended)

1. **Create the custom_templates directory** (if it doesn't exist):
   ```bash
   mkdir -p /config/custom_templates
   ```

2. **Download the template file** you want to use. For example:
   ```bash
   cd /config/custom_templates
   wget https://raw.githubusercontent.com/Bennett-Wendorf/home-assistant-template-utils/main/natural_language_parsing.jinja
   ```

3. **Configure Home Assistant** to use custom templates by adding this to your `configuration.yaml`:
   ```yaml
   homeassistant:
     customize: !include customize.yaml
     # Add the custom_templates path
     allowlist_external_dirs:
       - /config/custom_templates
   ```

4. **Import the template** in your automations, scripts, or template sensors:
   ```yaml
   # Example in a template sensor
   template:
     - sensor:
         - name: "Example Sensor"
           state: >
             {% from 'natural_language_parsing.jinja' import text_to_number %}
             {{ text_to_number("twenty-five") }}
   ```

5. **Restart Home Assistant** for the changes to take effect.

### Method 2: Using packages directory

If you use the packages feature:

1. Create a `templates` directory inside your packages directory:
   ```bash
   mkdir -p /config/packages/templates
   ```

2. Download the template files there:
   ```bash
   cd /config/packages/templates
   wget https://raw.githubusercontent.com/Bennett-Wendorf/home-assistant-template-utils/main/natural_language_parsing.jinja
   ```

3. Include the template in your package files using the relative path.

### Method 3: Inline in configuration

For smaller templates or testing, you can include them directly in your `configuration.yaml`:

```yaml
template:
  - trigger:
      - platform: state
        entity_id: sensor.some_sensor
    action:
      - service: notify.notify
        data:
          message: >
            {% from 'natural_language_parsing.jinja' import text_to_number %}
            The value is {{ text_to_number(trigger.to_state.state) }}
```

## 📖 Usage Examples

### Natural Language Number Parsing

```jinja
{% from 'natural_language_parsing.jinja' import text_to_number %}

{{ text_to_number("five") }}
{# Output: 5 #}

{{ text_to_number("twenty-three") }}
{# Output: 23 #}

{{ text_to_number("one hundred") }}
{# Output: 100 #}
```

### Relative Time Parsing

```jinja
{% from 'natural_language_parsing.jinja' import relative_time_to_minutes %}

{{ relative_time_to_minutes("in 5 minutes") }}
{# Output: 5 #}

{{ relative_time_to_minutes("in an hour") }}
{# Output: 60 #}

{{ relative_time_to_minutes("in 2 hours") }}
{# Output: 120 #}
```

### Time of Day Parsing

```jinja
{% from 'natural_language_parsing.jinja' import parse_time_of_day %}

{{ parse_time_of_day("noon") }}
{# Output: 12:00 #}

{{ parse_time_of_day("3pm") }}
{# Output: 15:00 #}

{{ parse_time_of_day("midnight") }}
{# Output: 00:00 #}
```

### Relative Date Parsing

```jinja
{% from 'natural_language_parsing.jinja' import parse_relative_date %}

{{ parse_relative_date("today") }}
{# Output: 2026-02-08 (current date) #}

{{ parse_relative_date("tomorrow") }}
{# Output: 2026-02-09 (next day) #}

{{ parse_relative_date("in 3 days") }}
{# Output: 2026-02-11 (3 days from now) #}
```

### Duration Parsing

```jinja
{% from 'natural_language_parsing.jinja' import duration_to_seconds %}

{{ duration_to_seconds("30 seconds") }}
{# Output: 30 #}

{{ duration_to_seconds("5 minutes") }}
{# Output: 300 #}

{{ duration_to_seconds("2 hours and 30 minutes") }}
{# Output: 9000 #}
```

### Real-World Automation Example

```yaml
automation:
  - alias: "Voice Assistant Timer"
    trigger:
      - platform: event
        event_type: voice_command
    action:
      - service: timer.start
        data:
          entity_id: timer.voice_timer
          duration: >
            {% from 'natural_language_parsing.jinja' import duration_to_seconds %}
            {{ duration_to_seconds(trigger.event.data.command) }}
```

## 🔧 Troubleshooting

### Templates not found
- Ensure the template file is in the correct directory
- Check that the path in your `from` statement matches the file location
- Verify Home Assistant has read permissions on the directory
- Restart Home Assistant after adding new template files

### Import errors
- Make sure you're using the correct macro name in your import statement
- Check that the template file has no syntax errors
- Review Home Assistant logs for detailed error messages

### Templates not updating
- After modifying a template file, restart Home Assistant
- Clear your browser cache if using the UI
- Check the Home Assistant logs for template reload errors

## 📝 Contributing

Contributions are welcome! If you have useful templates to share:

1. Fork this repository
2. Create your template file
3. Add documentation and examples
4. Submit a pull request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

These templates are designed to work with [Home Assistant](https://www.home-assistant.io/), an open-source home automation platform.

## 📚 Additional Resources

- [Home Assistant Templating Documentation](https://www.home-assistant.io/docs/configuration/templating/)
- [Jinja2 Documentation](https://jinja.palletsprojects.com/)
- [Home Assistant Community Forum](https://community.home-assistant.io/)
