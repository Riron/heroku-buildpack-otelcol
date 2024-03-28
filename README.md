# OpenTelemetry Collector Heroku Buildpack

Fork of https://github.com/Djiit/heroku-buildpack-otelcol
Forked as it must not start the collector in order to be used as a standalone collector.
Various adjustments:
- change otel default version
- default to collector contrib
- don't start the binary automatically

## Configuration

This buildpack assumes that otel collector config file is located at `/otelcol/config.yml` in your application.

In addition, you can include a prerun script, `/otelcol/prerun.sh`, in your application. 
The prerun script runs after all of the standard configuration actions and immediately before starting OpenTelemetry Collector. 
This allows you to modify the environment variables (for example: $DISABLE_OTELCOL), perform additional configurations, etc.

```shell
#!/usr/bin/env bash

# Disable based on dyno type
if [ "$DYNOTYPE" == "run" ]; then
  DISABLE_OTELCOL="true"
fi

# Update configuration placeholder using the Heroku application environment variable
if [ -n "$RUNTIME_URL" ]; then
  sed -i "s/<URL>/$RUNTIME_URL/" "$APP_OTELCOL/config.yml"
fi
```

## Credits

Most of the code and inspiration comes from the excellent [sendsonar's Prometheus Heroku Buildpack][4]

[1]: https://devcenter.heroku.com/articles/buildpacks
[2]: https://github.com/open-telemetry/opentelemetry-collector
[3]: https://github.com/open-telemetry/opentelemetry-collector-contrib
[4]: https://github.com/sendsonar/heroku-buildpack-prometheus


