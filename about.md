---
layout: article
titles:
  # @start locale config
  en      : &EN       Status
  en-GB   : *EN
  en-US   : *EN
  en-CA   : *EN
  en-AU   : *EN
  zh-Hans : &ZH_HANS  
  zh      : *ZH_HANS
  zh-CN   : *ZH_HANS
  zh-SG   : *ZH_HANS
  zh-Hant : &ZH_HANT  
  zh-TW   : *ZH_HANT
  zh-HK   : *ZH_HANT
  ko      : &KO       현황
  ko-KR   : *KO
  fr      : &FR       
  fr-BE   : *FR
  fr-CA   : *FR
  fr-CH   : *FR
  fr-FR   : *FR
  fr-LU   : *FR
  # @end locale config
key: page-about
---
# Competition Status

## Current Standings

{% assign users = site.data.progress | map: "user" | uniq %}
{% assign total_problems = site.data.progress | map: "problem_idx" | uniq | size %}
{% assign total_successes = site.data.progress | where: "succeed", "TRUE" | map: "problem_idx" | uniq | size %}

### Problems Attempted

{% for user in users %}
  {% assign user_problems = site.data.progress | where: "user", user | map: "problem_idx" | uniq | size %}
  {% assign percentage = user_problems | times: 100.0 | divided_by: total_problems %}
  {% assign full_hearts = percentage | divided_by: 10 | floor %}
  {% assign half_heart = percentage | modulo: 10 | divided_by: 5 | floor %}
  {% assign empty_hearts = 10 | minus: full_hearts | minus: half_heart %}
  
  {{ user }}: 
  {% for i in (1..full_hearts) %}❤️{% endfor %}
  {% if half_heart == 1 %}💔{% endif %}
  {% for i in (1..empty_hearts) %}🖤{% endfor %}
  ({{ user_problems }}/{{ total_problems }})
{% endfor %}

### Successful Submissions

{% for user in users %}
  {% assign user_successes = site.data.progress | where: "user", user | where: "succeed", "TRUE" | map: "problem_idx" | uniq | size %}
  {% assign percentage = user_successes | times: 100.0 | divided_by: total_successes %}
  {% assign full_hearts = percentage | divided_by: 10 | floor %}
  {% assign half_heart = percentage | modulo: 10 | divided_by: 5 | floor %}
  {% assign empty_hearts = 10 | minus: full_hearts | minus: half_heart %}
  
  {{ user }}: 
  {% for i in (1..full_hearts) %}❤️{% endfor %}
  {% if half_heart == 1 %}💔{% endif %}
  {% for i in (1..empty_hearts) %}🖤{% endfor %}
  ({{ user_successes }}/{{ total_successes }})
{% endfor %}

## Recent Activity Calendar

{% assign today = site.time | date: "%Y-%m-%d" %}
{% assign two_weeks_ago = today | date: "%s" | minus: 1209600 | date: "%Y-%m-%d" %}

{% for user in users %}
  {% assign user_color = user | hash | slice: 0, 6 %}
  <span style="color: #{{ user_color }};">■</span> {{ user }}
{% endfor %}

| Sun | Mon | Tue | Wed | Thu | Fri | Sat |
|-----|-----|-----|-----|-----|-----|-----|
{% for i in (0..13) %}
  {% assign day = two_weeks_ago | date: "%s" | plus: i | times: 86400 | date: "%Y-%m-%d" %}
  {% assign day_entries = site.data.progress | where: "date", day %}
  {% assign day_users = day_entries | group_by: "user" | sort: "size" | reverse %}
  {% assign winner = day_users | first %}
  {% if winner %}
    {% assign winner_color = winner.name | hash | slice: 0, 6 %}
    {% if ((forloop.index0 | modulo: 7)) == 0 %}<tr>{% endif %}
    <td style="background-color: #{{ winner_color }};">{{ day | date: "%-d" }}</td>
    {% if (forloop.index0 | modulo: 7) == 6 %}</tr>{% endif %}
  {% else %}
    {% if ((forloop.index0 | modulo: 7)) == 0 %}<tr>{% endif %}
    <td>{{ day | date: "%-d" }}</td>
    {% if (forloop.index0 | modulo: 7) == 6 %}</tr>{% endif %}
  {% endif %}
{% endfor %}

# How to Add Your Accomplishment

To add your accomplishment to the competition, follow these steps:

1. Fork the repository if you haven't already.
2. Navigate to the `_data` folder in your forked repository.
3. Open the `progress.csv` file.
4. Add a new row with the following information:
   - `user`: Your GitHub username
   - `date`: The date of your accomplishment (YYYY-MM-DD format)
   - `problem_idx`: The index or identifier of the problem you solved
   - `succeed`: Set to `TRUE` if you successfully solved the problem, `FALSE` otherwise
5. Commit your changes with a meaningful commit message.
6. Create a pull request to merge your changes into the main repository.

Example row in `progress.csv`:
```
user,date,problem_idx,succeed
johndoe,2024-08-23,problem42,TRUE
```

Once your pull request is reviewed and merged, the competition status will be automatically updated to reflect your accomplishment.

Good luck and happy coding!
