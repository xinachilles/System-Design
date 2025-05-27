
<https://youtu.be/yVnVY2HPVsI>

23 minute

I'll summarize the key points about API throttling from the document:

1. **Goals of Throttling**
   - Limit load by identity
   - Prevent "noisy neighbor" problems in multi-tenant systems
   - Protect system resources from overuse
   - Ensure fair resource distribution among users

2. **Throttling Mechanisms**
   - **Delay**: Slows down requests without failing them
     - First line of defense
     - Spreads out the load
     - Less disruptive to users
   - **Blocking**: Completely stops requests
     - More aggressive approach
     - Used when delay isn't sufficient
     - Returns 429 (Too Many Requests) status code

3. **Client Communication**
   - Response headers provide information about:
     - Current usage limits
     - Remaining quota
     - When limits will reset
     - Whether requests are being delayed or blocked
   - Helps clients make intelligent decisions about:
     - When to back off
     - How to adjust their request patterns
     - When to retry requests

4. **Implementation Features**
   - Highly configurable per service type
   - Different thresholds for different operations:
     - Work tracking
     - Version control
     - Release management
     - Code search
   - Tracks resource usage over time windows
   - Monitors various metrics (CPU, database time, etc.)

5. **Best Practices**
   - Start with delay before blocking
   - Provide clear feedback to clients
   - Allow clients to react intelligently
   - Use standard HTTP status codes (429)
   - Include detailed error messages

6. **Benefits**
   - Prevents system overload
   - Ensures fair resource distribution
   - Protects against accidental or malicious overuse
   - Maintains system stability
   - Allows for graceful degradation

This approach to throttling helps maintain system stability while providing a good user experience by:
- Being transparent about limits
- Giving clients the information they need to adapt
- Using progressive enforcement (delay before block)
- Being configurable for different use cases

We also need let people know when you hit the limit and when did you approaching the limit. What's the limit. how did you deal this with the user. We need be flexible enough to be able to response range issues.


![image.png](https://i.imgur.com/zeFmDQC.png)

