# Home Assistant Template Utils

A collection of Jinja macros and utilities for Home Assistant templates. These templates extend the functionality of Home Assistant's templating system with useful utilities for parsing natural language numbers, working with dates and times, and more.

## 📚 Available Templates

### Natural Language Parsing Numbers (`nlp_numbers.jinja`)

Parse natural language expressions for numbers in your Home Assistant automations and templates.

**Features:**
- Convert text numbers to integers ("twenty-five" → 25)
- Convert ordinal numbers to integers ("first" → 1)

### Natural Language Parsing Dates and Times (`nlp_dates_and_times.jinja`)

Parse natural language expressions for dates and times in your Home Assistant automations and templates.

**Features:**
- Convert relative time expressions to timedelta objects ("in 5 minutes" → timedelta(minutes=5))
- Convert relative date expressions to timedelta objects given a base date ("Monday" → timedelta(days=3))
- Convert absolute month expressions to month numbers ("January" → 1)
- Convert absolute date expressions to datetime objects in the future ("March 15" → date(2026, 3, 15))
- Convert absolute time expressions to datetime objects on a date ("3pm" → January 15th, 2026, 15:00)
- Convert a raw datetime string using any of the above formats to a datetime object ("in 5 minutes" → datetime object 5 minutes from now)

### Natural Language Dates and Times (`natural_language_dates_and_times.jinja`)

Output natural language expressions for dates and times in your Home Assistant automations and templates.

**Features:**
- Get the weekday index ("Monday" → 1)
- Output a datetime as a relative date ("2026-02-08" → "Today")
- Get the nearest date for a given weekday ("Monday" → 2026-02-09)
- Get the nearest date for a given weekday, inclusive of the current day ("Monday" → 2026-02-09 if today is Sunday, 2026-02-16 if today is Monday)

### Schedule (`schedule.jinja`)

Utilities for working with schedules in Home Assistant templates.

**Features:**
- Get the next scheduled time for a given schedule

### Cron (`cron.jinja`)

Utilities for working with cron patterns in Home Assistant templates

**Features:**
- Get the next time matching a cron pattern

## Installation

1. **Create the custom_templates directory** (if it doesn't exist):
   ```bash
   mkdir -p /config/custom_templates
   ```

2. **Download the template file(s)** you want to use into the `/config/custom_templates` directory.

3. **Reload Home Assistant** config or restart Home Assistant to recognize the new template files.

4. **Import the template** in your automations, scripts, or template sensors:
   ```yaml
   # Example in a template sensor
   template:
     - sensor:
         - name: "Example Sensor"
           state: >
             {% from 'nlp_numbers.jinja' import convert_written_number_to_digits %}
             {{ convert_written_number_to_digits("twenty-five") }}
   ```

## 🔧 Troubleshooting

### Templates not found
- Ensure the template file is in the correct directory
- Check that the path in your `from` statement matches the file location (you don't need a path if it's in the `custom_templates` directory)
- Restart Home Assistant after adding new template files

### Import errors
- Make sure you're using the correct macro name in your import statement
- Review Home Assistant logs for detailed error messages

## 📝 Contributing

Contributions are welcome! If you have useful templates to share:

1. Fork this repository
2. Create your template file
3. Add documentation and examples
4. Submit a pull request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

These templates are designed to work with [Home Assistant](https://www.home-assistant.io/), an open-source home automation platform.

## 📚 Additional Resources

- [Home Assistant Templating Documentation](https://www.home-assistant.io/docs/configuration/templating/)
- [Jinja2 Documentation](https://jinja.palletsprojects.com/)
- [Home Assistant Community Forum](https://community.home-assistant.io/)
