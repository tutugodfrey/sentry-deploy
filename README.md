helm template sentry sentry/sentry --include-crds --output-dir sentry/base/
helm template sentry sentry/sentry --nohooks --include-crds --output-dir sentry2/base/
