<script>
// I wrote this component in Option API style, so cyper can find the isBalanced method.
export default{
    data(){
        return {
            description: '',
            isBalancedText: true,
            balancedMessage: 'The text is balanced.',
            unbalancedMessage: 'The text is not balanced.',
            timeout: null
        }
    },
    methods: {
        validateDescription() {
            clearTimeout(this.timeout);
            this.timeout = setTimeout(() => {
                this.isBalancedText = this.isBalanced(this.description);
            }, 400);
        },
        isBalanced(str) {
            const stack = [];
            const pairs = { ')': '(', ']': '[', '}': '{' };
            for (const char of str){
                if (Object.values(pairs).includes(char)){
                    stack.push(char);
                } else if (Object.keys(pairs).includes(char)){
                    if (!stack.length || stack.pop() !== pairs[char]) {
                        return false;
                    }
                }
            }
            return stack.length === 0;
        }
    }
}
</script>

<template>
    <div class="flex flex-col space-y-4">
        <label for="description">Enter Text:</label>
        <input 
        class="border border-gray-300 px-4 py-2 rounded focus:ring-green-700 focus:ring-2 focus:outline-none"
        type="text"
        id="description"
        v-model="description"
        placeholder="Type some text here ..."
        @input="validateDescription"
        />
        <p v-if="description.length > 0" class="italic font-semibold" :class="{ 'text-green-700': isBalancedText, 'text-red-600': !isBalancedText }">{{ isBalancedText ? balancedMessage : unbalancedMessage }}</p>
    </div>
</template>