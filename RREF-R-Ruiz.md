
# Row Reduced Echelon Form in R

##### RREF review 

```
    J-G algorithm (https://www.di-mgt.com.au/matrixtransform.html)    
    set j to 1
    for each row i from 1 to n
    While column j has all zero elements, set j←j+1. If j>m return A. 
    If element aij is zero, then interchange row i with a row x>i that has axj≠0.
    Divide each element of row i by aij, thus making the pivot aij equal to one.
    For each row k from 1 to n, with k≠i, subtract row i multiplied by akj from row k.
    return transformed matrix A.
```


```R
#Create a randomized matrix of integers between -100 and 100 with size 200x200
randomMatrix <- matrix(sample((-100:100), size = 100000, replace = TRUE), nrow = 200, ncol = 200); randomMatrix
```


<table>
<caption>A matrix: 200 × 200 of type int</caption>
<tbody>
	<tr><td> 70</td><td> -63</td><td>-87</td><td>-51</td><td>  0</td><td>-99</td><td>-96</td><td>-24</td><td>-41</td><td>  27</td><td>...</td><td>-77</td><td>  35</td><td>  33</td><td> 44</td><td>-25</td><td> 38</td><td> 32</td><td>-41</td><td> -8</td><td> 92</td></tr>
	<tr><td>-83</td><td> -55</td><td>-17</td><td> 54</td><td> 55</td><td>-13</td><td>-68</td><td>-79</td><td> 63</td><td> -79</td><td>...</td><td>-75</td><td>  12</td><td> -47</td><td>  6</td><td>-35</td><td> 25</td><td>-19</td><td> 53</td><td>-93</td><td>-66</td></tr>
	<tr><td>-83</td><td>  27</td><td> 13</td><td>-78</td><td>-48</td><td>-51</td><td>-17</td><td> 58</td><td>-33</td><td>   7</td><td>...</td><td>  8</td><td>  61</td><td> -59</td><td> 86</td><td> 21</td><td>-36</td><td>-42</td><td>-13</td><td>-23</td><td> 83</td></tr>
	<tr><td> 90</td><td>   3</td><td>-38</td><td> 85</td><td> 27</td><td>-23</td><td>-15</td><td>-94</td><td> 19</td><td>  81</td><td>...</td><td> 54</td><td>  63</td><td>  31</td><td> 91</td><td> -6</td><td>-52</td><td>-15</td><td> 98</td><td>100</td><td> 51</td></tr>
	<tr><td> 12</td><td> -13</td><td> 98</td><td> 68</td><td>-50</td><td>-14</td><td>-43</td><td> 95</td><td> -5</td><td> -13</td><td>...</td><td> 29</td><td> -64</td><td>  -1</td><td>-71</td><td>-39</td><td> 22</td><td> 87</td><td> 35</td><td>-97</td><td>-12</td></tr>
	<tr><td> 48</td><td>   4</td><td> 78</td><td>-88</td><td> 42</td><td> -3</td><td> 42</td><td>-66</td><td>-27</td><td> -57</td><td>...</td><td>  8</td><td> 100</td><td> -33</td><td>-84</td><td> 81</td><td> 62</td><td> 28</td><td> 84</td><td>-31</td><td> 95</td></tr>
	<tr><td>-85</td><td>  57</td><td> 16</td><td>-51</td><td> 46</td><td> -1</td><td>-54</td><td> 19</td><td> 64</td><td>  74</td><td>...</td><td>-59</td><td>  64</td><td>  -5</td><td>-82</td><td>  9</td><td>-44</td><td> 65</td><td> 62</td><td> 61</td><td>-77</td></tr>
	<tr><td> 52</td><td>  74</td><td> 48</td><td>-36</td><td>-49</td><td>-90</td><td>-37</td><td> 74</td><td>-45</td><td>  15</td><td>...</td><td> 12</td><td>  15</td><td>  28</td><td> 99</td><td> 89</td><td>  6</td><td> 41</td><td> 40</td><td>-82</td><td>-55</td></tr>
	<tr><td>-36</td><td>  27</td><td> 49</td><td> 60</td><td>-38</td><td> 94</td><td> 23</td><td> 46</td><td>-24</td><td>  -1</td><td>...</td><td> 98</td><td> -52</td><td> -70</td><td>-57</td><td>-48</td><td> 81</td><td>-91</td><td>-25</td><td>-87</td><td>-96</td></tr>
	<tr><td> 80</td><td>  -5</td><td>-78</td><td> 35</td><td> 63</td><td> 47</td><td>  9</td><td>  9</td><td>-87</td><td>  18</td><td>...</td><td>-70</td><td>-100</td><td>-100</td><td>-67</td><td> 87</td><td> 96</td><td>-32</td><td> 93</td><td>-15</td><td>-49</td></tr>
	<tr><td> 20</td><td>  95</td><td>  0</td><td> 43</td><td> -3</td><td>-54</td><td>-72</td><td>-35</td><td> 39</td><td>  -1</td><td>...</td><td> 64</td><td> -99</td><td>  25</td><td>-21</td><td>-55</td><td>-36</td><td> 99</td><td>-81</td><td> 65</td><td> 62</td></tr>
	<tr><td>-55</td><td> -39</td><td>  6</td><td>-84</td><td> 93</td><td>-39</td><td>-29</td><td> 73</td><td>-71</td><td> -34</td><td>...</td><td> 65</td><td> -61</td><td>  63</td><td>-16</td><td> 28</td><td>-56</td><td> 81</td><td> 50</td><td> 90</td><td> 38</td></tr>
	<tr><td>-34</td><td> -35</td><td> -1</td><td> 54</td><td>-97</td><td>-81</td><td> 96</td><td>-49</td><td> 89</td><td> -15</td><td>...</td><td> 50</td><td> -68</td><td> -35</td><td>-78</td><td> 60</td><td> 40</td><td>-28</td><td> -2</td><td>-45</td><td>  1</td></tr>
	<tr><td> 73</td><td> -29</td><td> 19</td><td>-19</td><td>-43</td><td>-63</td><td> 65</td><td>-87</td><td> -7</td><td> -63</td><td>...</td><td>-66</td><td> -58</td><td> -82</td><td> 19</td><td> 69</td><td>-81</td><td>  7</td><td> 53</td><td>-81</td><td> 38</td></tr>
	<tr><td>-67</td><td>-100</td><td>-73</td><td> 90</td><td>-41</td><td>-42</td><td>-45</td><td>-93</td><td>  8</td><td>  96</td><td>...</td><td>-35</td><td>  65</td><td>   5</td><td>-82</td><td> 10</td><td> 46</td><td> -3</td><td>-94</td><td>-17</td><td> 23</td></tr>
	<tr><td>-66</td><td>  35</td><td> 31</td><td>-85</td><td>-69</td><td> 48</td><td>-79</td><td> 33</td><td>-38</td><td>  12</td><td>...</td><td>-65</td><td> -65</td><td> -11</td><td> 37</td><td> 20</td><td> 12</td><td>-40</td><td> 18</td><td>-19</td><td> 88</td></tr>
	<tr><td> 69</td><td>  88</td><td> -2</td><td> 36</td><td> 85</td><td> 20</td><td> 76</td><td> 42</td><td> 25</td><td>-100</td><td>...</td><td>-22</td><td> -45</td><td>-100</td><td> 93</td><td>-29</td><td>-47</td><td> 15</td><td> 99</td><td> 96</td><td> 74</td></tr>
	<tr><td> 89</td><td> -63</td><td>-45</td><td>-16</td><td> 78</td><td> 56</td><td> -9</td><td>-42</td><td> 75</td><td> -65</td><td>...</td><td> 85</td><td>  91</td><td> -86</td><td>-45</td><td>-56</td><td>  7</td><td> 13</td><td> 70</td><td>-90</td><td>-13</td></tr>
	<tr><td> 18</td><td>  31</td><td>-12</td><td>-54</td><td>-38</td><td> 86</td><td> 52</td><td> -8</td><td> 49</td><td> -82</td><td>...</td><td> 42</td><td>  11</td><td> -58</td><td>-29</td><td> 31</td><td>-70</td><td>-94</td><td> 95</td><td> -7</td><td> 69</td></tr>
	<tr><td> 23</td><td>-100</td><td>-77</td><td>-91</td><td>-59</td><td> 71</td><td> 59</td><td>-22</td><td>-23</td><td>  47</td><td>...</td><td> 27</td><td> -57</td><td> -10</td><td>-74</td><td> 96</td><td> 16</td><td> 61</td><td>-65</td><td> 23</td><td> 90</td></tr>
	<tr><td>-66</td><td>  96</td><td>-53</td><td> 74</td><td>-89</td><td>-62</td><td> -6</td><td> 35</td><td> -9</td><td> -66</td><td>...</td><td>  7</td><td> -76</td><td>  96</td><td> 83</td><td> 86</td><td> 52</td><td>-95</td><td>-10</td><td> 77</td><td>-71</td></tr>
	<tr><td>-54</td><td>  97</td><td> 67</td><td> 37</td><td>-79</td><td>-31</td><td>-28</td><td>-23</td><td>-44</td><td>  38</td><td>...</td><td>-67</td><td>  60</td><td> -60</td><td> 76</td><td> 75</td><td>-39</td><td> 82</td><td>-10</td><td>-10</td><td> 82</td></tr>
	<tr><td>  7</td><td> -48</td><td>-94</td><td> 70</td><td> 81</td><td>  4</td><td> 88</td><td> 51</td><td> 40</td><td> -98</td><td>...</td><td>-35</td><td>  76</td><td> -94</td><td> 27</td><td> 16</td><td> -7</td><td> 96</td><td> 46</td><td> 20</td><td>-80</td></tr>
	<tr><td>-64</td><td> -61</td><td>-58</td><td>-44</td><td>-86</td><td>-37</td><td> 35</td><td>-59</td><td> 23</td><td> -41</td><td>...</td><td>  0</td><td> -83</td><td>  49</td><td> 23</td><td> 35</td><td> 30</td><td> 76</td><td>-79</td><td> 21</td><td>-41</td></tr>
	<tr><td>-86</td><td>  -9</td><td>-57</td><td>-56</td><td> 12</td><td>-16</td><td>-95</td><td>-26</td><td> 70</td><td> -56</td><td>...</td><td> 21</td><td>  57</td><td>  57</td><td> 14</td><td> 46</td><td> 95</td><td>-66</td><td> 38</td><td>-41</td><td>-59</td></tr>
	<tr><td> 83</td><td>  47</td><td> 61</td><td> 82</td><td>-37</td><td> 93</td><td> 23</td><td>-12</td><td>-36</td><td>  46</td><td>...</td><td>-50</td><td> -61</td><td>  64</td><td>-68</td><td> 13</td><td> 15</td><td>-49</td><td> 34</td><td> 86</td><td> 46</td></tr>
	<tr><td> 70</td><td>  -6</td><td>-76</td><td> 98</td><td> 76</td><td>-69</td><td> 97</td><td>-64</td><td>-52</td><td> -53</td><td>...</td><td>-29</td><td>   0</td><td>  27</td><td> 79</td><td> 24</td><td> 94</td><td>-80</td><td> 23</td><td>-87</td><td>-21</td></tr>
	<tr><td> 63</td><td> -50</td><td>-60</td><td>-87</td><td>-96</td><td> -5</td><td> -1</td><td> 43</td><td>-89</td><td>  46</td><td>...</td><td> 95</td><td>  51</td><td>  56</td><td>-10</td><td>-76</td><td> 32</td><td>100</td><td> 45</td><td> 59</td><td> 83</td></tr>
	<tr><td> 47</td><td> -44</td><td> -5</td><td>-54</td><td> 84</td><td> 96</td><td> 57</td><td> 80</td><td>-96</td><td> -70</td><td>...</td><td>-76</td><td> -62</td><td>  77</td><td>-36</td><td> 52</td><td> 82</td><td>-59</td><td> 93</td><td> 15</td><td> 24</td></tr>
	<tr><td> 17</td><td> -39</td><td> 67</td><td> 81</td><td>  0</td><td>-19</td><td>-18</td><td> 89</td><td>-32</td><td>  55</td><td>...</td><td>-83</td><td>  79</td><td> -82</td><td>-81</td><td> 40</td><td> 61</td><td> 61</td><td> 85</td><td>-81</td><td>-56</td></tr>
	<tr><td>...</td><td>...</td><td>...</td><td>...</td><td>...</td><td>...</td><td>...</td><td>...</td><td>...</td><td>...</td><td></td><td>...</td><td>...</td><td>...</td><td>...</td><td>...</td><td>...</td><td>...</td><td>...</td><td>...</td><td>...</td></tr>
	<tr><td>-58</td><td>  5</td><td>-63</td><td> 53</td><td>-77</td><td> 36</td><td> 35</td><td> 45</td><td> 45</td><td> 51</td><td>...</td><td>-36</td><td>-60</td><td> 90</td><td> 31</td><td>-83</td><td> -50</td><td> 93</td><td>  1</td><td>-95</td><td> 44</td></tr>
	<tr><td>-25</td><td>-84</td><td>-38</td><td>-99</td><td> 35</td><td> -7</td><td> 81</td><td> -2</td><td> 17</td><td> 33</td><td>...</td><td>-34</td><td>-61</td><td>-22</td><td>-61</td><td>-18</td><td>  48</td><td> 91</td><td> 79</td><td>-68</td><td> 20</td></tr>
	<tr><td> 95</td><td> 84</td><td>-42</td><td>-58</td><td> -6</td><td> 16</td><td>-96</td><td>-18</td><td> 50</td><td>-44</td><td>...</td><td>-94</td><td> 32</td><td>-97</td><td>  1</td><td> -7</td><td> -77</td><td> 77</td><td>-11</td><td>-18</td><td> 35</td></tr>
	<tr><td> 16</td><td>-92</td><td> 89</td><td> 95</td><td>-53</td><td>-14</td><td>  2</td><td> -9</td><td> 34</td><td> 63</td><td>...</td><td> 24</td><td>-72</td><td>-85</td><td>  9</td><td> 54</td><td>   2</td><td>-53</td><td>-82</td><td> 67</td><td> 21</td></tr>
	<tr><td>-66</td><td> 41</td><td>-31</td><td>-33</td><td> 28</td><td> -2</td><td>-89</td><td>-76</td><td> 23</td><td>-51</td><td>...</td><td> 81</td><td>-84</td><td>-34</td><td> 82</td><td>-83</td><td> -47</td><td> 39</td><td> 60</td><td>-17</td><td> 29</td></tr>
	<tr><td>-46</td><td> 40</td><td>-66</td><td> 23</td><td> 10</td><td>-30</td><td> 51</td><td>-69</td><td>-69</td><td> 70</td><td>...</td><td>-44</td><td>-53</td><td>-89</td><td>-78</td><td>  4</td><td> -32</td><td> 17</td><td> 99</td><td>-33</td><td>-60</td></tr>
	<tr><td> 34</td><td>-45</td><td>-20</td><td>-21</td><td> 17</td><td>-36</td><td>-54</td><td>  1</td><td>-51</td><td> -7</td><td>...</td><td>-45</td><td>-70</td><td> 35</td><td> 18</td><td> 29</td><td> -14</td><td>-87</td><td>-20</td><td>-56</td><td>-76</td></tr>
	<tr><td> 72</td><td> 79</td><td>-70</td><td>-95</td><td>  6</td><td>-60</td><td> 34</td><td> 34</td><td> 25</td><td> 56</td><td>...</td><td>-39</td><td>-38</td><td>-37</td><td> -8</td><td>-25</td><td> -81</td><td> 40</td><td> 15</td><td>-24</td><td> 78</td></tr>
	<tr><td>-41</td><td>-55</td><td>-36</td><td>-43</td><td>-13</td><td>-15</td><td> 91</td><td> -6</td><td>-51</td><td> 28</td><td>...</td><td>-18</td><td>-16</td><td> 89</td><td>-39</td><td> -8</td><td>  -1</td><td>-63</td><td> 49</td><td> 38</td><td>-30</td></tr>
	<tr><td> 59</td><td> 78</td><td> 85</td><td>-67</td><td>-83</td><td> 40</td><td> 56</td><td> 14</td><td>-47</td><td>-18</td><td>...</td><td>-30</td><td>-22</td><td>-27</td><td> -4</td><td> 26</td><td> -58</td><td>-47</td><td> 22</td><td> 47</td><td>-64</td></tr>
	<tr><td>-11</td><td> 22</td><td>-28</td><td>-98</td><td> 91</td><td> 66</td><td> -1</td><td>-48</td><td> 80</td><td> 46</td><td>...</td><td>  3</td><td>  2</td><td> 22</td><td> 45</td><td>  8</td><td> -47</td><td> 81</td><td>-41</td><td> 55</td><td> 83</td></tr>
	<tr><td>-37</td><td>-35</td><td> 25</td><td> 10</td><td>-27</td><td>-45</td><td>-66</td><td>-40</td><td> 17</td><td>-62</td><td>...</td><td> 59</td><td> 36</td><td>-59</td><td>-91</td><td> 94</td><td>  15</td><td> 91</td><td> 69</td><td> 59</td><td>-22</td></tr>
	<tr><td>  3</td><td> 21</td><td>-97</td><td>-30</td><td> 73</td><td>-32</td><td> 21</td><td>-28</td><td>-16</td><td>-48</td><td>...</td><td> 32</td><td> 36</td><td> 48</td><td> 88</td><td>-58</td><td>  -1</td><td> 33</td><td>-82</td><td> 78</td><td>-50</td></tr>
	<tr><td>-57</td><td>-45</td><td>  8</td><td> 35</td><td>-55</td><td>  0</td><td> 71</td><td>-15</td><td> 68</td><td>-59</td><td>...</td><td>-43</td><td>-65</td><td>-20</td><td>-88</td><td>-75</td><td>  61</td><td> -8</td><td>-72</td><td>-39</td><td>-55</td></tr>
	<tr><td> 66</td><td>-35</td><td>-32</td><td> 20</td><td>-43</td><td> -6</td><td> 91</td><td> 83</td><td> 54</td><td>-49</td><td>...</td><td> 82</td><td>-80</td><td>-69</td><td> 14</td><td> 77</td><td>  28</td><td> 44</td><td>-99</td><td> 72</td><td> 99</td></tr>
	<tr><td>  4</td><td>-69</td><td>-65</td><td>-28</td><td> 19</td><td> 90</td><td> 29</td><td>-71</td><td> -2</td><td> 91</td><td>...</td><td> 48</td><td> 88</td><td>-70</td><td>  9</td><td>-61</td><td>  39</td><td> -2</td><td> 96</td><td>-32</td><td>-12</td></tr>
	<tr><td>-99</td><td>-32</td><td>-63</td><td> 45</td><td>-49</td><td> 52</td><td> 46</td><td>  4</td><td>  7</td><td> 50</td><td>...</td><td>-25</td><td> 63</td><td> 13</td><td> -9</td><td>-24</td><td>   0</td><td> 22</td><td> 43</td><td> 81</td><td>-53</td></tr>
	<tr><td>-18</td><td> 90</td><td>-51</td><td>  4</td><td>-92</td><td> 77</td><td>  3</td><td> 13</td><td>-89</td><td>  5</td><td>...</td><td>-66</td><td>-18</td><td>-98</td><td>-97</td><td>  3</td><td>  -5</td><td> 13</td><td>-56</td><td> 16</td><td>-49</td></tr>
	<tr><td>-71</td><td> 80</td><td> 92</td><td> 55</td><td> 24</td><td> 87</td><td>  1</td><td> 17</td><td> 42</td><td> 64</td><td>...</td><td>-92</td><td>-53</td><td> 40</td><td>  9</td><td>-65</td><td>  98</td><td> 38</td><td>-31</td><td> 85</td><td> 12</td></tr>
	<tr><td>-64</td><td>-95</td><td> -7</td><td> 39</td><td>-16</td><td>-21</td><td> 74</td><td> 25</td><td>-34</td><td> 54</td><td>...</td><td> 26</td><td>-23</td><td> 31</td><td> 19</td><td> 59</td><td>  14</td><td> 96</td><td>-34</td><td>-74</td><td> 95</td></tr>
	<tr><td> 83</td><td>-93</td><td>-36</td><td> 40</td><td>-42</td><td> 28</td><td> 82</td><td> 56</td><td> 72</td><td> 64</td><td>...</td><td>-75</td><td>-56</td><td>-22</td><td> 69</td><td>-52</td><td>  -6</td><td>-62</td><td>-60</td><td>100</td><td>-69</td></tr>
	<tr><td>-39</td><td>-33</td><td>-25</td><td>-45</td><td>-26</td><td> 32</td><td> 71</td><td>-26</td><td>-24</td><td>100</td><td>...</td><td>  3</td><td> 66</td><td>-93</td><td> 99</td><td> 83</td><td> -34</td><td> 90</td><td> 13</td><td>-84</td><td> 28</td></tr>
	<tr><td> 80</td><td>-55</td><td>-71</td><td> 84</td><td> 29</td><td>-34</td><td>-85</td><td>-30</td><td> 12</td><td> 95</td><td>...</td><td> 90</td><td> 52</td><td>-85</td><td> 19</td><td>-65</td><td> -36</td><td>-85</td><td> -4</td><td>-70</td><td> 20</td></tr>
	<tr><td>-23</td><td> -1</td><td>-61</td><td>-60</td><td>-27</td><td> 52</td><td> 81</td><td> 28</td><td> 57</td><td>-23</td><td>...</td><td>-47</td><td>-91</td><td> 10</td><td>-24</td><td>-72</td><td> -96</td><td>-27</td><td>  2</td><td> 99</td><td> 16</td></tr>
	<tr><td> 11</td><td>  1</td><td> 51</td><td> 91</td><td> 45</td><td> 86</td><td> -1</td><td>-85</td><td>-33</td><td>-88</td><td>...</td><td> 98</td><td> 92</td><td>-34</td><td> 51</td><td> 13</td><td> -26</td><td>-82</td><td> 84</td><td>-46</td><td>-59</td></tr>
	<tr><td>-10</td><td> 82</td><td> 41</td><td> 98</td><td>-53</td><td> 60</td><td> 42</td><td>-65</td><td>-17</td><td>-55</td><td>...</td><td> 90</td><td>-33</td><td>-20</td><td> 95</td><td> 27</td><td>-100</td><td>  6</td><td>-56</td><td>-78</td><td> 98</td></tr>
	<tr><td>-91</td><td> 10</td><td>-65</td><td> 36</td><td>-86</td><td>-57</td><td>-12</td><td> -4</td><td> 44</td><td> 28</td><td>...</td><td> 30</td><td> 17</td><td> 48</td><td> 53</td><td> 83</td><td>  88</td><td> 79</td><td>100</td><td>-58</td><td>-20</td></tr>
	<tr><td>-75</td><td>-46</td><td>-35</td><td>-10</td><td> 31</td><td> 46</td><td> 27</td><td>-75</td><td>-67</td><td> 78</td><td>...</td><td>-52</td><td> 21</td><td>-59</td><td>-35</td><td> 70</td><td> -68</td><td> 96</td><td>-82</td><td>-64</td><td> 19</td></tr>
	<tr><td>-85</td><td>-39</td><td>-76</td><td> 42</td><td>-71</td><td>  9</td><td> 78</td><td> -7</td><td> 45</td><td> 16</td><td>...</td><td> -8</td><td> 82</td><td> 34</td><td>-36</td><td>-77</td><td> -87</td><td>-89</td><td> -9</td><td> 55</td><td> 65</td></tr>
	<tr><td> 67</td><td> 69</td><td> -7</td><td>-47</td><td>-16</td><td>-55</td><td>-18</td><td>-97</td><td> 75</td><td> 12</td><td>...</td><td>-95</td><td>-10</td><td> -4</td><td>-66</td><td>-88</td><td> -27</td><td>-30</td><td> 93</td><td>  7</td><td> -6</td></tr>
</tbody>
</table>




```R
RREF <- function(mat) {
  rowSize <- nrow(mat)
  colSize <- ncol(mat)
  temp <- matrix(0)
  i <- 1
  j <- 1
  
  for (i in 1:(rowSize)) {
    for (j in 1:(colSize)) {
      if (mat[i, j] == 0) {
        # scan rows with "leading 0s"
        scanRow <- i
        temp <- mat[i, ]
        
        # while there is a "leading zero" for a row, scan next row
        while (scanRow < (rowSize + 1) && mat[scanRow, j] == 0) {
          scanRow <- scanRow + 1
        }
        
        # if there are leading zeros in every row of first column
        # move to next column (j)
        if (scanRow == (rowSize + 1)) {
          j <- j + 1
          next
        }
        
        # Swap rows
        mat[i, ] = mat[scanRow, ]  # Scanned row without "leading zero"
        mat[scanRow, ] = temp   # Interchange rows
      }
      
      
      # Divide each element of row i by aij, thus making the pivot aij equal to one.
      mat[i, j:colSize] = (mat[i, j:colSize] / mat[i, j])
      
      # Scan remaining rows to force remaining pivots
      for (newRow in 1:(rowSize)) {
        if (newRow == i) {
          next # Iterate for all rows in matrix
        }
        
        if (mat[newRow, j] != 0) {
          # For each row newRow from 1 to n, with newRow ≠i, subtract row i multiplied by a(newRow)j from row newRow.
          for (x in 1:length(mat[i, ])) {
            mat[newRow, ] = mat[newRow, ] -  mat[newRow, j] * mat[i, ]
          }
        }
      }
      
      i <- i + 1
      j <- j + 1
            
      if (i >= (rowSize + 1) ||
          j >= (colSize + 1)) {
        # Exit loop once mat dims reached
        break
      }
      
    }
    
    return(mat)
  }
}
```

    Your code contains a unicode char which cannot be displayed in your
    current locale and R will silently convert it to an escaped form when the
    R kernel executes this code. This can lead to subtle errors if you use
    such chars to do comparisons. For more information, please see
    https://github.com/IRkernel/repr/wiki/Problems-with-unicode-on-windows


```R
RREF(randomMatrix)
```


<table>
<caption>A matrix: 200 × 200 of type dbl</caption>
<tbody>
	<tr><td>1</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>1</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>1</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>1</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>1</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>1</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>1</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>1</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>1</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>1</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>...</td><td>...</td><td>...</td><td>...</td><td>...</td><td>...</td><td>...</td><td>...</td><td>...</td><td>...</td><td></td><td>...</td><td>...</td><td>...</td><td>...</td><td>...</td><td>...</td><td>...</td><td>...</td><td>...</td><td>...</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>1</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>1</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>1</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>1</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>1</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>1</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>1</td><td>0</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>1</td><td>0</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>1</td><td>0</td></tr>
	<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>1</td></tr>
</tbody>
</table>


