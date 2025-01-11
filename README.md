# Big-Three

class BigThreeTest {

    public static void main(String args[]) {


	System.out.println(BigThree.add_base('1', '2'));
	System.out.println(BigThree.add_carry('1', '2'));


	BigThree s0 = new BigThree();			// decimal : 0
	BigThree s5 = new BigThree("12");		// decimal : 5
	BigThree s14 = new BigThree("112");		// decimal : 14
	BigThree s20 = new BigThree("202");		// decimal : 20
	BigThree s1715 = new BigThree("2100112");	// decimal : 1715
	BigThree s1899 = new BigThree("2121100");	// decimal : 1899
	BigThree s31741831 = new BigThree("2012201122121101");	// decimal : 31741831
	BigThree s58927148 = new BigThree("11002212210211222");	// decimal : 58927148

	BigThree r1 = s5.MultiplyDivideAndConquer(s0);
	System.out.println("Result (should be 0) : " + r1.Str());
	
	BigThree r30 = s5.MultiplyDivideAndConquer(s31741831);
	System.out.println("Result (should be 102001122021000212) : " + r30.Str());

    BigThree r31 = s20.MultiplyDivideAndConquer(s1899);
	System.out.println("Result (should be 1221002200) : " + r31.Str());

	BigThree r2 = s14.Add(s20);
	System.out.println("Result (should be (1021) : " + r2.Str());	

	BigThree r3 = s14.Add(s1715);
	System.out.println("Result (should be (2101001) : " + r3.Str());	

	BigThree r4 = s1715.Add(s20);
	System.out.println("Result (should be (2101021) : " + r4.Str());	

	BigThree r5 = s1715.Add(s1899);
	System.out.println("Result (should be (11221212) : " + r5.Str());	

	BigThree r6 = s20.Subtract(s14);
	System.out.println("Result (should be (20) : " + r6.Str());	

	BigThree r7 = s1899.Subtract(s20);
	System.out.println("Result (should be (2120121) : " + r7.Str());	

	BigThree r8 = s31741831.Subtract(s1715);
	System.out.println("Result (should be (2012201120020212) : " + r8.Str());	

	BigThree r9 = s58927148.Subtract(s31741831);
	System.out.println("Result (should be (1220011011020121) : " + r9.Str());	

	BigThree r12 = s14.MultiplyDivideAndConquer(s20);
	System.out.println("Result (should be (101101) : " + r12.Str());	

	BigThree r13 = s14.MultiplyDivideAndConquer(s1715);
	System.out.println("Result (should be (1012221021) : " + r13.Str());	

	BigThree r14 = s1899.MultiplyDivideAndConquer(s1715);
	System.out.println("Result (should be (20010110110200) : " + r14.Str());	

	BigThree r15 = s31741831.MultiplyDivideAndConquer(s31741831);
	System.out.println("Result (should be (11220010102020202020001111222201) : " + r15.Str());	

	BigThree r21 = BigThree.Power10(4);
	System.out.println("Result (should be (111201101) : " + r21.Str());	
	
	BigThree r29 = BigThree.Power10(9);
	System.out.println("Result (should be (2120200200021010001) : " + r29.Str());	
	

	BigThree r22 = BigThree.BigThreeFromDecimal(17246109);
	System.out.println("Result (should be (1012110012012210) : " + r22.Str());	

    BigThree r88 = BigThree.BigThreeFromDecimal(172461099);
	System.out.println("Result (should be (110000111221011010) : " + r88.Str());	
	

	}


}
public class BigThree {

    private String number; // This will store the base-3 number as a string

    // Default Constructor
    public BigThree() {
        this.number = "0";
    }

    // String Constructor
    public BigThree(String s) {
        if (isValidBaseThree(s)) {
            this.number = s;
        } else {
            this.number = "0";
        }
    }

    // Copy Constructor
    public BigThree(BigThree bt) {
        this.number = bt.number;
    }

    // Helper method to check if the given string is a valid base-3 number
    private boolean isValidBaseThree(String s) {
        for (char c : s.toCharArray()) {
            if (c != '0' && c != '1' && c != '2') {
                return false;
            }
        }
        return true;
    }

// Add method: Adds the current BigThree with another BigThree and returns the result
public BigThree Add(BigThree s1) {
    StringBuilder result = new StringBuilder();
    String num1 = this.number;
    String num2 = s1.number;

    // Pad the shorter string with leading zeros
    int lengthDiff = Math.abs(num1.length() - num2.length());
    if (num1.length() < num2.length()) {
        num1 = "0".repeat(lengthDiff) + num1;
    } else if (num2.length() < num1.length()) {
        num2 = "0".repeat(lengthDiff) + num2;
    }

    int carry = 0;

    // Add digits from right to left
    for (int i = num1.length() - 1; i >= 0; i--) {
        int digit1 = num1.charAt(i) - '0';
        int digit2 = num2.charAt(i) - '0';

        // Add the digits and the carry
        int sum = digit1 + digit2 + carry;

        // Update the carry for next iteration
        carry = sum / 3;

        // Append the current digit to the result
        result.append(sum % 3);
    }

    // If there's a carry left, append it
    if (carry > 0) {
        result.append(carry);
    }

    // Reverse the result to get the final sum
    return new BigThree(result.reverse().toString());
}


   // Subtract method: Subtracts another BigThree from the current BigThree and returns the result
public BigThree Subtract(BigThree s1) {
    StringBuilder result = new StringBuilder();
    String num1 = this.number;
    String num2 = s1.number;

    // Pad the shorter string with leading zeros
    int lengthDiff = Math.abs(num1.length() - num2.length());
    if (num1.length() < num2.length()) {
        num1 = "0".repeat(lengthDiff) + num1;
    } else if (num2.length() < num1.length()) {
        num2 = "0".repeat(lengthDiff) + num2;
    }

    int borrow = 0;

    // Subtract digits from right to left
    for (int i = num1.length() - 1; i >= 0; i--) {
        int digit1 = num1.charAt(i) - '0';
        int digit2 = num2.charAt(i) - '0' + borrow;

        if (digit1 < digit2) {
            digit1 += 3;
            borrow = 1;
        } else {
            borrow = 0;
        }

        result.append(digit1 - digit2);
    }

    // Remove leading zeros
    while (result.length() > 1 && result.charAt(result.length() - 1) == '0') {
        result.deleteCharAt(result.length() - 1);
    }

    // Reverse the result to get the final difference
    return new BigThree(result.reverse().toString());
}

    // MultiplyDivideAndConquer method: Multiplies the current BigThree with another BigThree using divide and conquer
public BigThree MultiplyDivideAndConquer(BigThree s1) {
    String num1 = this.number;
    String num2 = s1.number;

    // Base case: if either number is "0", return "0"
    if (num1.equals("0") || num2.equals("0")) {
        return new BigThree("0");
    }

    // Base case: if either number is a single digit, perform direct multiplication
    if (num1.length() == 1 && num2.length() == 1) {
        int product = (num1.charAt(0) - '0') * (num2.charAt(0) - '0');
        return BigThreeFromDecimal(product);
    }

    // Ensure both numbers are of equal length by padding
    int maxLength = Math.max(num1.length(), num2.length());
    if (num1.length() < maxLength) {
        num1 = "0".repeat(maxLength - num1.length()) + num1;
    }
    if (num2.length() < maxLength) {
        num2 = "0".repeat(maxLength - num2.length()) + num2;
    }

    // Split the numbers into halves
    int mid = maxLength / 2;

    BigThree high1 = new BigThree(num1.substring(0, mid));
    BigThree low1 = new BigThree(num1.substring(mid));
    BigThree high2 = new BigThree(num2.substring(0, mid));
    BigThree low2 = new BigThree(num2.substring(mid));

    // Recursively calculate the four products
    BigThree z0 = low1.MultiplyDivideAndConquer(low2);
    BigThree z1 = (low1.Add(high1)).MultiplyDivideAndConquer(low2.Add(high2));
    BigThree z2 = high1.MultiplyDivideAndConquer(high2);

    // Calculate the result using the three products
    BigThree part1 = z2.Shift(2 * (maxLength - mid));
    BigThree part2 = z1.Subtract(z2).Subtract(z0).Shift(maxLength - mid);
    BigThree result = part1.Add(part2).Add(z0);

    // Remove leading zeros from the final result
    String resultStr = result.Str();
    while (resultStr.length() > 1 && resultStr.charAt(0) == '0') {
        resultStr = resultStr.substring(1);
    }

    return new BigThree(resultStr);
}

    // Shift method: Shifts the BigThree number by adding k zeros at the end
    public BigThree Shift(int k) {
        if (k <= 0) {
            return this;
        }
        String shiftedNumber = this.number + "0".repeat(k);
        return new BigThree(shiftedNumber);
    }

    // Static method to add two base-3 digits
    public static char add_base(char x, char y) {
        int sum = (x - '0') + (y - '0');
        return (char) ('0' + (sum % 3));
    }

    // Static method to determine if adding two base-3 digits results in a carry
    public static boolean add_carry(char x, char y) {
        int sum = (x - '0') + (y - '0');
        return sum >= 3;
    }

    // Static method to subtract two base-3 digits
    public static char sub_base(char x, char y) {
        int diff = (x - '0') - (y - '0');
        if (diff < 0) {
            diff += 3;
        }
        return (char) ('0' + diff);
    }

    // Static method to determine if subtracting two base-3 digits results in a borrow
    public static boolean sub_carry(char x, char y) {
        return (x - '0') < (y - '0');
    }

    // Static method to multiply two base-3 digits
    public static char multi_base(char x, char y) {
        int val1 = x - '0';
        int val2 = y - '0';
        int product = val1 * val2;
        return (char) ('0' + (product % 3));
    }

    // Static method to calculate 10 to the power of n in base 3
    public static BigThree Power10(int k) {
    if (k == 0) {
        return new BigThree("1");
    } else if (k == 1) {
        return new BigThree("101");
    }

    BigThree baseTernary = convertDecimalToTernary((int) Math.pow(10, k));
    return baseTernary;
}

private static BigThree convertDecimalToTernary(int decimal) {
    if (decimal == 0) {
        return new BigThree("0");
    }
    StringBuilder ternary = new StringBuilder();
    while (decimal > 0) {
        int remainder = decimal % 3;
        ternary.insert(0, remainder);
        decimal /= 3;
    }
    return new BigThree(ternary.toString());
}

    // Static method to convert a decimal number to a base-3 BigThree object
    public static BigThree BigThreeFromDecimal(int decimal) {
        StringBuilder base3 = new StringBuilder();
        if (decimal == 0) {
            return new BigThree("0");
        }
        while (decimal > 0) {
            base3.append(decimal % 3);
            decimal /= 3;
        }
        return new BigThree(base3.reverse().toString());
    }

    // Method to get the number as a string representation
    public String Str() {
        return this.number;
    }
}
