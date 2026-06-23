## Swapchain-image-ready semaphore

In wsi.cpp the semaphore `acquire` is passed to `vkAcquireNextImageKHR()` is later stored a `wsi` object via
`set_acquire_semaphore()`. This	`acquire` semaphore is then passed to `VkSubmitInfo` in through
`composer.add_wait_semaphore()`(device.cpp).
