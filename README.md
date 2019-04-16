# Sentry client for zap logger

Forked from https://github.com/TheZeroSlave/zapsentry 

Added support for Go modules and few other changes.

Integration of sentry client into zap.Logger is pretty simple:
```golang
func modifyToSentryLogger(log *zap.Logger, DSN string) *zap.Logger {
	cfg := zapsentry.Configuration{
		Level: zapcore.ErrorLevel, //when to send message to sentry
		Tags: map[string]string{
			"component": "system",
		},
	}
	core, err := zapsentry.NewCore(cfg, zapsentry.NewSentryClientFromDSN(DSN))
	//in case of err it will return noop core. so we can safely attach it
	if err != nil {
		log.Warn("failed to init zap")
	}
	return zapsentry.AttachCoreToLogger(core, log)
}
```
