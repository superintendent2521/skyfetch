# skyfall usage guide

## Quick start

```python
from skyfall import SkyFall

client = SkyFall(api_key="your-openweather-key")
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

- `metric` - Celsius temperatures (default)
- `imperial` - Fahrenheit temperatures and miles-per-hour wind speed
- `standard` - Kelvin temperature, meters-per-second wind speed

```python
SkyFall(api_key="...", units="imperial")
```

## Timeouts

Pass a `timeout` keyword argument to guard against long-running requests. Defaults to 10 seconds.

```python
SkyFall(api_key="...", timeout=5)
```

## Handling errors

- Invalid input raises `ValueError`.
- Network issues or API errors raise `SkyFallError`.

Wrap calls in a `try`/`except` block if you want custom behaviour:

```python
from skyfall import SkyFall, SkyFallError

client = SkyFall(api_key="...")

try:
    weather = client.weather("Rome")
except SkyFallError as exc:
    print(f"Could not fetch weather: {exc}")
```

## Troubleshooting

- *401 Unauthorized*: Check the API key and make sure it is activated.
- *404 Not Found*: City name was not recognized; try including country codes (e.g. `"Paris,FR"`).
- *429 Too Many Requests*: You hit the rate limit. Upgrade your OpenWeatherMap plan or wait before retrying.
