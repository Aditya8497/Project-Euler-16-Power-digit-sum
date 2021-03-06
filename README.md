# Project-Euler-16-Power-digit-sum
#include <stdio.h>

#define size_of_number 3330 //2^10000 < 8^3333 < 10^3333. So I've set the size at 3330
#define base 10

//Finding num^{exponent}
void find_power(int number[], int num, int exponent)
{
    int i, first_digit;
    int carry, replace, product, power_counter;

    //First digit keeps track of the position of the 'most significant digit' in the array - where the trailing zeroes end.
    first_digit = 0;
    number[first_digit] = 1; //If we don't do this, the answer will be 0.

    //This while loop calculates num! and stores it in number[]
    for(power_counter = 1; power_counter <= exponent; power_counter++)
    {
        //This block performs multiplication of number[] with the current value of num and stores the result in num
        //The multiplication is done like how we normally do it by hand.
        carry = 0;
        for(i = 0; i <= first_digit; i++)
        {
            product = num*number[i] + carry;
            replace = product%base; //Replace is what should be rewritten in the ith digit
            carry = product/base; //Carry needs to be added when num is multiplied with the next most significant digit

            //The ith digit of number is rewritten now as the product%base we're working in. This case - 10
            number[i] = replace;

            //If there is a carry in the MSB, then the number of digits will increase. For example, 112x10 = 1120. There is a carry in the MSB.
            if( (i == first_digit) && (carry > 0) )
            {
                first_digit++;
            }
        }

    }
}

//Finding the sum of all digits
int sum_of_digits(int number[])
{
    int i, sum = 0;

    //All the digits are initialised to zero so we don't need to know where MSB is to avoid extra terms being added
    for(i = 0; i < size_of_number; i++)
    {
        sum = sum + number[i];
    }
    return sum;
}

void solve()
{
    int number[size_of_number] = {0}, sum, exponent, num = 2;
    scanf("%d", &exponent);

    find_power(number, num, exponent);
    sum = sum_of_digits(number);
    printf("%d\n",sum);
}

int main()
{
    int no_of_queries;
    scanf("%d", &no_of_queries);
    while(no_of_queries-- != 0)
        solve();

    return 0;}
