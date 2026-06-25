## Swapchain-image-ready semaphore

In wsi.cpp the semaphore `acquire` is passed to `vkAcquireNextImageKHR()` and is later stored to a `wsi` object via `set_acquire_semaphore()`. This	`acquire` semaphore is then passed to `VkSubmitInfo` through `composer.add_wait_semaphore()`(device.cpp).

## Render-complete semaphore

See comments in `WSI::end_frame()` above the line `release_semaphores[swapchain_index] = std::move(release);`.

## Frame fences

Frame fences are requested and recycled in `Device::end_frame_nolock()` which is called in `Device::next_frame_context()` before `frame().begin()`. The fences are waited on in `Device::PerFrame::wait()` at the beginning of `Device::PerFrame::begin()`. The flow is:
1. Request a unset fence.
2. Pass it to `vkQueueSubmit2()`.
3. Add the fence to the list to be waited on.
4. Wait for fences in `Device::PerFrame::begin()`. There is nothing to wait for the first frame.
5. Record commands.
6. Go back to the step 1. 
