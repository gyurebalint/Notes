---------------------------------------------------RUST - LANGUAGE------------------------------------------------------------------
RESOURCES 
Rust Programming Course for Beginners - Tutorial - FreeCodeCamp - https://www.youtube.com/watch?v=MsocPEZBd-M
Visualizing memory layout of Rust's data types - https://www.youtube.com/watch?v=rDoqT-a6UFg&t=0s


https://doc.rust-lang.org/book/ch03-01-variables-and-mutability.html
# BASICS  
## VARIABLES AND MUTABILITY  
		Variables: immutable by default. You can opt out of this by using 	let mut x = 5;
		Constants: always immutable. Type must be annotated					const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
		Shadowing: you can declare a new variable with the same name as a previous variable. Rustaceans say that the first 
					variable is shadowed by the second, which means that the second variable is what the compiler will see when you use the name of the variable.
					fn main() {
						let x = 5;

						let x = x + 1;

						{
							let x = x * 2;
							println!("The value of x in the inner scope is: {x}");
						}

						println!("The value of x is: {x}");
					}			
					This program first binds x to a value of 5. Then it creates a new variable x by repeating let x =, taking the original value 
					and adding 1 so the value of x is then 6. Then, within an inner scope created with the curly brackets, the third let statement 
					also shadows x and creates a new variable, multiplying the previous value by 2 to give x a value of 12. When that scope is over,
					the inner shadowing ends and x returns to being 6. When we run this program, it will output the following:
					The value of x in the inner scope is: 12
					The value of x is: 6
					
					Different from mut: have to use the let keyword again. After these transformations, it then becomes immutable
										type agnostic: 	let spaces = "     ";
														let spaces = spaces.len(); NO ERRORS
	DATA TYPES
	To data type subsets: scalar and compound.

		SCALAR TYPES
		Rust has four primary scalar types: integers, floating-point numbers, Booleans, and characters
			INTEGERS
			Length	Signed	Unsigned
			8-bit	i8	u8
			16-bit	i16	u16
			32-bit	i32	u32
			64-bit	i64	u64
			128-bit	i128	u128
			arch	isize	usize
			Signed and unsigned refer to whether it’s possible for the number to be negative—in other words, whether the number needs to have a sign with it 
			(signed) or whether it will only ever be positive and can therefore be represented without a sign (unsigned)
			
			Each signed variant can store numbers from -(2n - 1) to 2n - 1 - 1 inclusive, where n is the number of bits that variant uses. 
			So an i8 can store numbers from -(27) to 27 - 1, which equals -128 to 127. 

			Unsigned variants can store numbers from 0 to 2n - 1, so a u8 can store numbers from 0 to 28 - 1, which equals 0 to 255.

			FLOATING-POINT TYPES
			fn main() {
				let x = 2.0; // f64

				let y: f32 = 3.0; // f32
			}
			
			THE BOOLEAN TYPE
			fn main() {
				let t = true;

				let f: bool = false; // with explicit type annotation
			}
			
			THE CHARACTER TYPE
			fn main() {
				let c = 'z';
				let z: char = 'ℤ'; // with explicit type annotation
				let heart_eyed_cat = '😻';
			}
			Accented letters; Chinese, Japanese, and Korean characters; emoji; and zero-width spaces are all valid char values in Rust

		COMPOUND TYPES
		Compound types can group multiple values into one type. Rust has two primitive compound types: tuples and arrays.
			
			THE TUPLE TYPE
			Tuples have a fixed length: once declared, they cannot grow or shrink in size.
			We create a tuple by writing a comma-separated list of values inside parentheses.
			
			fn main() {
				let tup: (i32, f64, u8) = (500, 6.4, 1);
			}

				ACCESSING A TUPLE : DESTRUCTURING
				fn main() {
					let tup = (500, 6.4, 1);

					let (x, y, z) = tup;

					println!("The value of y is: {y}");
				}
				ACCESSING A TUPLE : .index
				fn main() {
					let x: (i32, f64, u8) = (500, 6.4, 1);

					let five_hundred = x.0;

					let six_point_four = x.1;

					let one = x.2;
				}
			The tuple without any values has a special name, unit. This value and its corresponding type are both written () 
			and represent an empty value or an empty return type. Expressions implicitly return the unit value if they don’t return any other value.
			
			THE ARRAY TYPE
			Unlike a tuple, every element of an array must have the same type. Unlike arrays in some other languages, arrays in Rust have a fixed length.
			let a: [i32; 5] = [1, 2, 3, 4, 5];
			let first = a[0];
			let second = a[1];
			let sixth = a[6] : results in a 'index out of bounds' at RUNTIME
			
	FUNCTIONS
		fn another_function(x: i32) {
			println!("The value of x is: {x}");
		}
		Expression: evaluates to a value. Do NOT have ';' at the end of the line.
		Statement: not return a value

		fn main() {
			let y = {
				let x = 3;
				x + 1
			};

			println!("The value of y is: {y}");
		}
		y evaluates to 4.

		FUNCTION WITH RETURN VALUES
		In Rust, the return value of the function is synonymous with the value of the final expression in the block of the body of a function. 
		You can return early from a function by using the return keyword and specifying a value, but most functions return the last expression implicitly.
		fn five() -> i32 {
			5
		}

		fn main() {
			let x = five();

			println!("The value of x is: {x}");
		}
	COMMENTS
		//This is a comment

	REPEATING CODE
		LOOPS
		The loop keyword tells Rust to execute a block of code over and over again forever or until you 
		explicitly tell it to stop.
		fn main() {
			loop {
				println!("again!");
			}
		}
		Fortunately, Rust also provides a way to break out of a loop using code. You can place the break 
		keyword within the loop to tell the program when to stop executing the loop. 

		RETURNING VALUES WITH LOOPS
		fn main() {
			let mut counter = 0;

			let result = loop {
				counter += 1;

				if counter == 10 {
					break counter * 2;
				}
			};

			println!("The result is {result}");
		}
		Finally, we print the value in result, which in this case is 20.

		LOOP LABELS TO DISAMBIGUATE BETWEEN MULTIPLE LOOPS
		If you have loops within loops, break and continue apply to the innermost loop at that point. You
		can optionally specify a loop label on a loop that we can then use with break or continue to specify 
		that those keywords apply to the labeled loop instead of the innermost loop. Loop labels must begin 
		with a single quote. Here’s an example with two nested loops:

		fn main() {
			let mut count = 0;
			'counting_up: loop {
				println!("count = {count}");
				let mut remaining = 10;

				loop {
					println!("remaining = {remaining}");
					if remaining == 9 {
						break;
					}
					if count == 2 {
						break 'counting_up;
					}
					remaining -= 1;
				}

				count += 1;
			}
			println!("End count = {count}");
		}

		WHILE
		fn main() {
			let a = [10, 20, 30, 40, 50];
			let mut index = 0;

			while index < 5 {
				println!("the value is: {}", a[index]);

				index += 1;
			}
		}
		All five array values appear in the terminal, as expected. Even though index will reach a value of 5 at
		some point, the loop stops executing before trying to fetch a sixth value from the array.

		However, this approach is error prone; we could cause the program to panic if the index value or 
		test condition are incorrect. For example, if you changed the definition of the a array to have 
		four elements but forgot to update the condition to while index < 4, the code would panic. It’s 
		also slow, because the compiler adds runtime code to perform the conditional check of whether the 
		index is within the bounds of the array on every iteration through the loop.

		FOR
		fn main() {
			let a = [10, 20, 30, 40, 50];

			for element in a {
				println!("the value is: {element}");
			}
		}
		When we run this code, we’ll see the same output as in Listing 3-4. More importantly, we’ve now 
		increased the safety of the code and eliminated the chance of bugs that might result from going 
		beyond the end of the array or not going far enough and missing some items.
		Using the for loop, you wouldn’t need to remember to change any other code if you changed the 
		number of values in the array, as you would with the method used in Listing 3-4.
		The safety and conciseness of for loops make them the most commonly used loop construct in Rust.
		Even in situations in which you want to run some code a certain number of times, as in the 
		countdown example that used a while loop in Listing 3-3, most Rustaceans would use a for loop. 
		The way to do that would be to use a Range, provided by the standard library, which generates 
		all numbers in sequence starting from one number and ending before another number.
		fn main() {
			for number in (1..4).rev() {
				println!("{number}!");
			}
			println!("LIFTOFF!!!");
		}

UNDERSTANDING OWNERSHIP

WHAT IS OWNERSHIP
Ownership is a set of rules that governs how a Rust program manages memory. 
All programs have to manage the way they use a computer’s memory while running. 
	- Some languages have garbage collection that regularly looks for no-longer used memory as the program runs; 
	- In other languages, the programmer must explicitly allocate and free the memory. 
	- Rust uses a third approach: memory is managed through a system of ownership with a set of rules that the compiler checks. 
		If any of the rules are violated, the program won’t compile. 
		None of the features of ownership will slow down your program while it’s running.
		




























 
MEMORY MANAGEMENT
	GarbageCollection languages:
		when 	int v1 = new Sth();
				v2 = v1;
		v2 pointer points to the same address v1 is pointing at.
	In rust:
		v2 becomes the owner of v1 -> you can't use v1 anymore. It gets cleaned up when v2 goes out of scope.
		
	clone: explicitly have to tell this should happen. this wastes memory on heap
		Copies the variable, creates new memory allocation for heap.

	Rc smart pointer: with .clone() - Reference Counted Pointer
		variable on stack, points to a smart pointer on the heap containing how many references are there which points to the
		actual information on heap. This is shared ownership, the memory is only deallocated if all of the owners go out of scope.
		The inside information (memory allocated in heap) is immutable

	Arc: Atomically Reference pointer
		Shared references across thrads. It costs extra (that is why it is an independent type). Arc is immutable.
		If you want to mutate the data: Arc<Mutex<i32>> = Arc::new(Mutex::new(0)) if one of the threads want to change the data,
		it has to aquire the lock first (only one thread at a time can have the lock)

	Trait:
		Consists of a fat pointer (aka two pointers)
			- a pointer to the value
			- pointer to the virtual table or vtable :vtable is generated once at compile time and shared by all objects of the same type
			
	Function pointer
		A variable that stores the reference of a function

	Clojures
