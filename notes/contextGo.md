# Context in Go

Context acts like a deadline for a process to execute else return

`Background` and `TODO` contexts return an new empty context. They should be used on top levels and sparingly. Todo is useful for debugging purposes.

`context.WithValue` creates a dervied context adding a KVP to the provided context. This value is available to any further derived contexts and flow with the context

`context.WithCancel` creates a derived context with a cancel function. Only the function that creates this should call the cancel function to cancel this context. Never pass around the `cancel` function. It should be called from within the function it was created else confusion may arise if `cancel` is **called in an unexpected place**

`context.WithDeadline` creates a derived context which gets cancelled when the provided deadline ends or the cancel function is called. This can be used to tackle `WithCancel` issues. Cancel can be safely passed around without worries as the deadline will already cancel within the required time hence no after affects can be produced. `context.WithTimeout` behaves in a similar manner. It just takes a duration instead of a time object.
