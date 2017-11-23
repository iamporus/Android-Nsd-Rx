# Android Network Service Discovery Rx
An Rx wrapper around the Android [NsdManager](https://developer.android.com/reference/android/net/nsd/NsdManager.html) api.

## Creating a Manager
The NsdManagerRx can be created with a context instance.

```kotlin
NsdManagerRx.fromContext(context)
```

### Discovery
The service name used is defined in rfc6763 and finds all services without filtering.
[ietf.org/rfc/rfc6763.txt](http://www.ietf.org/rfc/rfc6763.txt)

```kotlin
NsdManagerRx(context)
    .discoverServices(DiscoveryConfiguration("_services._dns-sd._udp"))
    .subscribe({event: DiscoveryEvent -> Log.d("Discovery", "${event.service.serviceName}")})
```

### Register
```kotlin
NsdManagerRx(context)
    .registerService(RegistrationConfiguration(port = 12345))
    .subscribe({event: RegistrationEvent ->
        Log.d("Registration", "Registered ${event.nsdServiceInfo.serviceName}")
    })
```

### Resolve
```kotlin
NsdManagerRx(context)
    .resolveService(serviceInfo)
    .subscribe({event: ResolveEvent ->
        Log.d("Resolve", "Resolved ${event.nsdServiceInfo.serviceName}")
    })
```

## Binders
Discovery and registration return binders in their events. The binders allow the internal listener to be unregistered without disposing the observable. This is important if you want to receive the unregister and stop events from the Android NsdManager. Alternately, disposing of the observables will unregister the internal listeners.
