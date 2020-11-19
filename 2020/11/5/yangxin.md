# Keeping Netflix Reliable Using Prioritized Load Shedding
reason why Failure can occur 
- misbehaving clients that trigger a retry storm
- an under-scaled service in the backend
- a bad deployment
- a network blip 
- or issues with the cloud provider

## Building a request taxonomy
将请求流量分类
- NON_CRITICAL 不影响播放体验，但是吞吐量很大 log等
- DEGRADED_EXPERIENCE 影响体验，但是不影响播放
- CRITICAL 影像播放

## Finding the best place to throttle traffic
- Service throttling
- Global throttling

# Validating which requests are right for the job
- 模拟request错误进行试验，检查哪些流量降级不影响
