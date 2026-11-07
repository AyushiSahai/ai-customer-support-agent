# Project 2 Reflection: AI Customer Support Agent

## Design Decisions

I built a customer support agent using Amazon Bedrock AgentCore and the Strands SDK. I separated its responsibilities into specialized tools: AgentCore Gateway for order tracking and refund processing, Amazon Bedrock Knowledge Bases for retrieving support information, AgentCore Memory for retaining customer preferences across sessions, Code Interpreter for loyalty discount calculations, and AgentCore Browser for live web access. This modular architecture makes the agent easier to test and extend.

## Challenges and Solutions

One major challenge occurred during loyalty discount calculations. The Code Interpreter returned an EventStream response rather than a directly serializable dictionary. I investigated the SDK response structure, updated the parser to extract the result event, and added fallback handling. I tested both successful execution and the fallback path locally, then deployed the updated code. A live invocation returned the loyalty breakdown, and CloudWatch confirmed that the `calculate_loyalty_discount` tool was called.

Another challenge involved cross-session memory retrieval. I corrected the memory callback to use the message supplied by `MessageAddedEvent`. After deployment, I tested the same customer ID with two different session IDs. The agent successfully recalled a delivery notification preference saved in the first session.

## Production Considerations

For production use, I would introduce stronger customer authentication and authorization, tenant isolation, least-privilege IAM permissions, and secure Gateway authorization. I would also add refund idempotency, input validation, retries, monitoring alarms, and human escalation for sensitive or unsuccessful support requests. These improvements would make the agent more reliable, secure, and suitable for real customer workloads.