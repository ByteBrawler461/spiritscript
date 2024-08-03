# spiritscript
script
import json
import os

class Recipe:
    def __init__(self, name, ingredients, instructions):
        self.name = name
        self.ingredients = ingredients
        self.instructions = instructions

    def __str__(self):
        return f"Recipe: {self.name}\nIngredients: {', '.join(self.ingredients)}\nInstructions: {self.instructions}"

class RecipeBook:
    def __init__(self, filename="recipes.json"):
        self.filename = filename
        self.recipes = []
        self.load_recipes()

    def load_recipes(self):
        if os.path.exists(self.filename):
            with open(self.filename, "r") as file:
                recipes_data = json.load(file)
                self.recipes = [Recipe(**data) for data in recipes_data]
        else:
            self.recipes = []

    def save_recipes(self):
        with open(self.filename, "w") as file:
            json.dump([recipe.__dict__ for recipe in self.recipes], file, indent=4)

    def add_recipe(self, name, ingredients, instructions):
        new_recipe = Recipe(name, ingredients, instructions)
        self.recipes.append(new_recipe)
        self.save_recipes()

    def remove_recipe(self, name):
        self.recipes = [recipe for recipe in self.recipes if recipe.name != name]
        self.save_recipes()

    def edit_recipe(self, old_name, new_name, new_ingredients, new_instructions):
        for recipe in self.recipes:
            if recipe.name == old_name:
                recipe.name = new_name
                recipe.ingredients = new_ingredients
                recipe.instructions = new_instructions
                self.save_recipes()
                break

    def display_recipes(self):
        if not self.recipes:
            print("No recipes found.")
        else:
            for recipe in self.recipes:
                print(recipe)
                print("-" * 30)

def main():
    recipe_book = RecipeBook()

    while True:
        print("\nRecipe Book Menu")
        print("1. Add recipe")
        print("2. Remove recipe")
        print("3. Edit recipe")
        print("4. View recipes")
        print("5. Exit")

        choice = input("Enter your choice: ")

        if choice == "1":
            name = input("Enter recipe name: ")
            ingredients = input("Enter ingredients (comma separated): ").split(', ')
            instructions = input("Enter cooking instructions: ")
            recipe_book.add_recipe(name, ingredients, instructions)
        elif choice == "2":
            name = input("Enter the name of the recipe to remove: ")
            recipe_book.remove_recipe(name)
        elif choice == "3":
            old_name = input("Enter the name of the recipe to edit: ")
            new_name = input("Enter new recipe name: ")
            new_ingredients = input("Enter new ingredients (comma separated): ").split(', ')
            new_instructions = input("Enter new cooking instructions: ")
            recipe_book.edit_recipe(old_name, new_name, new_ingredients, new_instructions)
        elif choice == "4":
            recipe_book.display_recipes()
        elif choice == "5":
            break
        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()
