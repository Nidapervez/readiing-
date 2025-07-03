💡 What is Streaming in Agents SDK?
Streaming ka matlab hai:
✅ Agent jab kaam kar raha hota hai, uske har step ki live updates lena.
✅ Tum bina wait kiye real-time bata sakti ho user ko ke abhi agent kya kar raha hai (tool chala, message banaya, etc).

🛠 Important Methods / Concepts (Yaad Rakhne Wale Points)
Term/Method	Meaning (Easy Words)
Runner.run_streamed()	Streaming mode start karne ke liye use hota hai.
.stream_events()	Yeh ek event stream deta hai jisme har update (event) ko tum pakar sakti ho.
Event Types	Har event ka type hota hai jise check karte hain:
raw_response_event	LLM ka har word/token jese jese aata hai uska update deta hai (fine-grained).
run_item_stream_event	Jab agent koi kaam complete karta hai (tool chalta hai, message banta hai).
agent_updated_stream_event	Jab agent ka role/instructions change hote hain.

📋 Basic Flow (Asaan Zubaan Mein)
Tum Runner.run_streamed() se agent ko start karti ho.

Tum async for event in result.stream_events() likhti ho.

Har event ko check karna hota hai:

Agar tool chala → print

Agar message bana → print

Agar koi raw token aya → ignore ya print

✅ Quick Example (Yeh Basic Yaad Rakho)
python
Copy
Edit
import asyncio
from agents import Agent, Runner, function_tool, ItemHelpers
import random

@function_tool
def give_number() -> int:
    return random.randint(1, 5)

async def main():
    agent = Agent(
        name="Joker",
        instructions="Call give_number and tell that many jokes.",
        tools=[give_number],
    )

    result = Runner.run_streamed(agent, input="Hello")

    print("=== Run started ===")

    async for event in result.stream_events():
        if event.type == "run_item_stream_event":
            if event.item.type == "tool_call_output_item":
                print(f"Tool Output: {event.item.output}")
            elif event.item.type == "message_output_item":
                print(ItemHelpers.text_message_output(event.item))

    print("=== Run finished ===")

asyncio.run(main())
🔑 Easy Yaad Rakhne Ka Formula:
Cheez	Matlab
Runner.run_streamed()	Streaming run start karna
stream_events()	Updates ka flow
raw_response_event	Har token ka update (slow but detailed)
run_item_stream_event	Kaam complete hone ka update
agent_updated_stream_event	Agent ki update
ItemHelpers.text_message_output()	Message ko clean text mein dikhana

💭 Useful Tips:
Agar har word chahiye → raw_response_event use karo.

Agar sirf kaam complete chahiye → run_item_stream_event use karo.

Tum har event ka type check kar ke decide kar sakti ho ke usko print karna hai ya nahi.

Summary: Har Cheez Short Mein
Cheez	Matlab
Runner.run_streamed(agent, input)	Streaming agent start karta hai
result.stream_events()	Har naya event deta hai
raw_response_event	Word-by-word updates
run_item_stream_event	Kaam complete hone par updates
"tool_call_item"	Tool call ho raha hai
"tool_call_output_item"	Tool complete ho gaya (output ready)
"message_output_item"	Final message jo user ko milega
agent_updated_stream_event	Jab agent khud badal jaye











