package machine

val printSate = listOf("water", "milk", "coffee beans", "disposable cups", "money")
val printFill = listOf("ml of water", "ml of milk", "grams of coffee beans", "disposable cups of coffee")
val coffeeType = listOf(listOf(250, 0, 16, 1, 4), listOf(350, 75, 20, 1, 7), listOf(200, 100, 12, 1, 6))

enum class State(var message: String) {
    CHOSE_ACTION("Write action (buy, fill, take, remaining, exit): "),
    CHOSE_COFFEE("What do you want to buy? 1 - espresso, 2 - latte, 3 - cappuccino: "),
    FILL_MACHINE("Write how many ml of water do you want to add: ");
    companion object {
        var fillIndex = 0
        var currentState = CHOSE_ACTION
        fun increaseFillIndex() {
            println("Write how many ${printFill[fillIndex ++]} do you want to add: ")
        }
        fun toAction(): Boolean {
            currentState = CHOSE_ACTION.apply { println("\n${message}") }
            return true
        }
    }
}

fun main() {
    println("Write action (buy, fill, take, remaining, exit): ")
    while (true) { if (!CoffeeMachine.execute(readLine()!!)) break }
}

object CoffeeMachine {
    var properties: MutableList<Int> = mutableListOf(400, 540, 120, 9, 550)
    fun checkResources(it: Int): Boolean {
        for (i in 0 until properties.lastIndex) {
            if (properties[i] < coffeeType[it][i]) {
                println("Sorry, not enough ${printSate[i]}")
                return false
            }
        }
        println("I have enough resources, making you a coffee!")
        return true
    }

    fun execute(input: String): Boolean {
        when (State.currentState) {
            State.CHOSE_ACTION -> {
                when (input) {
                    "fill" -> {
                        State.currentState = State.FILL_MACHINE
                        println(State.currentState.message)
                    }
                    "buy" -> {
                        State.currentState = State.CHOSE_COFFEE
                        print(State.currentState.message)
                    }
                    "take" -> { take(); return State.toAction() }
                    "remaining" -> { state(); return State.toAction() }
                    "exit" -> return false
                }
                return true
            }
            State.CHOSE_COFFEE -> {
                buy(input)
                return State.toAction()
            }
            State.FILL_MACHINE -> {
                properties[State.fillIndex] += input.toInt()
                return if (State.fillIndex < 3) {
                    State.increaseFillIndex()
                    true
                } else {
                    State.fillIndex = 0
                    State.toAction()
                }
            }
        }
    }

    fun buy(input: String) {
        ((input.toIntOrNull() ?: 0) - 1).let {
            if (it != -1 && checkResources(it)) {
                for (i in 0 until properties.lastIndex) { properties[i] -= coffeeType[it][i] }
                properties[4] += coffeeType[it][4]
            }
        }
    }

    fun take() { println("I gave you \$${properties[4]}").also { properties[4] = 0 } }

    fun state() { for ((i, elem) in properties.withIndex()) { println("$elem of ${printSate[i]}") } }
}
