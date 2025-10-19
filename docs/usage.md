# sky_fetch usage guide

## Quick start

```python
from sky_fetch import SkyFetch

client = SkyFetch(api_key="your-openweather-key")
report = client.weather("Sydney")

print(report)
```

`WeatherReport` is a dataclass, so you can treat it like a lightweight object:

```python
print(report.city)
print(report.temperature_c)
print(report.raw["wind"])
```

## Units

OpenWeatherMap supports three unit systems:

- `metric` — Celsius temperatures (default)
- `imperial` — Fahrenheit temperatures and miles-per-hour wind speed
- `standard` — Kelvin temperature, meters-per-second wind speed

```python
SkyFetch(api_key="...", units="imperial")
```

## Timeouts

Pass a `timeout` keyword argument to guard against long-running requests. Defaults to 10 seconds.

```python
SkyFetch(api_key="...", timeout=5)
```

## Handling errors

- Invalid input raises `ValueError`.
- Network issues or API errors raise `SkyFetchError`.

Wrap calls in a `try`/`except` block if you want custom behaviour:

```python
from sky_fetch import SkyFetch, SkyFetchError

client = SkyFetch(api_key="...")

try:
    weather = client.weather("Rome")
except SkyFetchError as exc:
    print(f"Could not fetch weather: {exc}")
```

## Troubleshooting

- *401 Unauthorized*: Check the API key and make sure it is activated.
- *404 Not Found*: City name was not recognized—try including country codes (e.g. `"Paris,FR"`).
- *429 Too Many Requests*: You hit the rate limit. Upgrade your OpenWeatherMap plan or wait before retrying.
