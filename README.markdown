# Headspring Enumeration #


### Basic Usage ###

    public class Color : Enumeration<Color, int>
    {
        public static Color Red = new Color(1, "Red");
        public static Color Blue = new Color(2, "Blue");
        public static Color Green = new Color(3, "Green");

        public Color(int value, string displayName) : base(value, displayName) { }
    }

    public void Using_the_color_example()
    {
        Color myFavorite = Color.Blue;

        Color leastFavorite = Color.Green;
    }



    public class Color : Enumeration<Color, string>
    {
        public static Color Red = new Color("RED", "Red");
        public static Color Blue = new Color("BLUE", "Blue");
        public static Color Green = new Color("GREEN", "Green");

        public Color(string value, string displayName) : base(value, displayName) { }
    }

    public void Using_the_color_example()
    {
        Color myFavorite = Color.Blue;

        Color leastFavorite = Color.Green;
    }

### Creating a select list ###

    public class State : Enumeration<State, int>
    {
        public static State Alabama = new State(1, "AL", "Alabama");
        public static State Alaska = new State(2, "AK", "Alaska");
        // .. many more
        public static State Wyoming = new State(3, "WY", "Wyoming");

        public State(int value, string displayName, string description) : base(value, displayName)
        {
            Description = description;
        }

        public string Description { get; private set; }
    }

    public IEnumerable<SelectListItem> Creating_a_select_list(State selected)
    {
        return State.GetAll().Select(
            x => new SelectListItem
            {
                Selected = x == selected,
                Text = x.Description,
                Value = x.Value.ToString()
            });
    }

### As dispatch table ###
    
    public class Calculation : Enumeration<Calculation, int>
    {
        public static Calculation Add = new Calculation(1, "Add", (left, right) => left + right);
        public static Calculation Subtract = new Calculation(2, "Subtract", (left, right) => left - right);
        public static Calculation Multiply = new Calculation(3, "Multiply", (left, right) => left * right);

        public Calculation(int value, string displayName, Func<int, int, int> calculation)
            : base(value, displayName)
        {
            Go = calculation;
        }

        public Func<int, int, int> Go { get; private set; }
    }

    public int Using_the_calculator(Calculation input)
    {
        return input.Go(2, 4);
    }

### Helpful query methods ###

    public class Role : Enumeration<Role, int>
    {
        public static readonly Role System = new Role(999, "System", "System", true);
        public static readonly Role Manager = new Role(1, "Manager", "Michelle", false);
        public static readonly Role Employee = new Role(2, "Employee", "Eric", false);
        public static readonly Role HR = new Role(3, "Human Resources", "Harry", false);

        public Role(int value, string displayName, string personaName, bool testrole)
            : base(value, displayName)
        {
            PersonaName = personaName;
            TestRole = testrole;
        }

        public string PersonaName { get; private set; }
        public bool TestRole { get; private set; }

        public static IEnumerable<Role> GetAllProductionRoles()
        {
            return GetAll().Where(r => !r.TestRole);
        }
    }