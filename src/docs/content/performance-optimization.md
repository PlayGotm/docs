# Performance optimization

Avoid using many contents. For example, instead of storing inventory and stats in two different contents, store them in the same content.

Avoid saving content with every change. There is a limit to how often you can save content.

Use create_local when the content shouldn't or doesn't need to be accessible to other players or devices.
Use is_private when the content shouldn't be accessible to other players, but needs to be accessible to other devices.

<!-- Add matrix further clarifying local/private -->
