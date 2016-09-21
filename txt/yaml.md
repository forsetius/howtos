# YAML
### Generate a reference
```
mammal: &mammal_ref
    warm_blooded: true
    lays_eggs: false
```

### Define via reference assignment
```
beaver: *mammal_ref
```

# Define including a hash merge
```
otter:
    cute: true
    <<: *mammal_ref
```

# Define including a hash merge, overriding a value in the reference
```
platypus:
    <<: *mammal_ref
    lays_eggs: true
```
