/**
*  Student Number: 214429476
*  Assignment Number: 3
*  Name: Eric Lin
*  Assignment Name: Phylogenetic Selection
*  Interaction:
*        Left click to select up to two parent organisms
*        Press space bar to create the next generation (two parents must be selected)
*        Press the enter key to reset everything (restart from scratch)
**/ 

/**
*  Initialization and Declaration of variables and constants
*  changing these parameters will strongly affect emergent behaviour
**/
// A string that contains all the possible nucleotides for the organism
let lexicon = "Ff+-<>(.".split("")
// The population of organisms per generation
let pop = []
// The size of the population per generation
let pop_size = 16
// The scale of this value determines the chance that a nucleotide in the organism's DNA strand will mutate
let mutability = 1
// Uses the population size to calculate the number of columns and rows
let cols = Math.ceil(Math.sqrt(pop_size))
let rows = cols
// This variable represents the DNA string that will control the organism's movement
let cmds
let parent1Selected = false;
let parent2Selected = false;
let parent1Index = -1;
let parent2Index = -1;

/**
*  This function creates an organism
*  @param  size  The number of nucleotides in the DNA strand
**/
function create(size) 
{
  // An array that contains the organism's genomes
  let geno = []
  // The organisms will have random genomes on creation
  for (let i=0; i<size; i++) 
  {
    geno.push( lexicon[random(lexicon.length)] )  
  }
  return geno;
}

/**
*  This function resets/recreates the entire population
**/
function reset() 
{
  // An array containing all of the organisms in the current population
  pop = []
  // Creates a set of organisms to fill the population size
  for (let i=0; i<pop_size; i++) 
  {
    pop[i] = 
    {
      cmds: create(40),
    }
  }
}

/**
*  This function draws the turtle organism
*  @param  t     The turtle to be drawn
*  @param  cmds  The organism's DNA strand
**/
function turtledraw(t, cmds) 
{
    // The lines to be drawn
    let lines = [];
    
    // Goes through each of the nucleotides in the DNA strand
    for (let i=0; i<cmds.length; i++) 
    {
      // Retrieves the current nucleotide
      let cmd = cmds[i];
      // The movement and lines depends on the nucleotide
      // Different nucleotides result in different appearances
      if (cmd == "F") 
      {
        // A forward moving line
        lines.push(t.pos.clone());  
        t.pos.add(t.dir) // move
        lines.push(t.pos.clone());
      } 
      else if (cmd == "f") 
      {
        // A forward moving line (scale 0.5)
        lines.push(t.pos.clone());  
        t.pos.add(t.dir.clone().mul(0.5))
        lines.push(t.pos.clone());
      }
      else if (cmd == "+") 
      {
        // Rotate 90 degrees (+ direction)
        t.dir.rotate(t.angle * Math.sin(now))
      } 
      else if (cmd == "-") 
      {
        // Rotate 90 degrees (- direction)
        t.dir.rotate(-t.angle * Math.sin(now))
      } 
      else if (cmd == "<") 
      {
        // Scales the angle by 2
        t.angle *= 2
      } 
      else if (cmd == ">") 
      {
        // Scales the angle by 1/2
        t.angle /= 2
      } 
      else if (cmd == "(") 
      {
        // Duplicates the turtle
        let t1 = 
        {
          pos: t.pos.clone(),
          dir: t.dir.clone(),
          angle: -t.angle,
        }
        let morelines = turtledraw(t1, cmds.slice(i+1))
        lines = lines.concat(morelines)
      }
    }
    return lines;
}

/**
*  This function draws the turtle organism
*  @param  chosen  The organism selected by the observer
**/
function regenerate(chosen1, chosen2) 
{
  // The new populations
  let newpop = []
  // Uses the selected organism as the parent for the next generation
  let parent1 = pop[chosen1]
  let parent2 = pop[chosen2]
  
  // Goes through each of the organisms in the new population
  for (let i=0; i<pop_size; i++) 
  {
    // Creates a new organism
    let child = 
    {
      cmds: []
    }
    
    // The DNA strand of the parent is used as the base DNA strand for the child
    for (let j=0; j<parent1.cmds.length; j++) 
    {
      // If the random chance of mutation is less than the threshold, the current nucleotide will mutate
      // This is done to allows new elements to (re)emerge into the population
      // This is doen to prevent the population from stagnating
      if (random() < mutability / parent1.cmds.length) 
      {
        child.cmds[j] = lexicon[random(lexicon.length)]
      } 
      // Otherwise the current nucleotide remains the same
      else 
      {
        // The child's appearance/movements will be selected from either parent
        // Chance from inheriting from each parent is equal
        let randParentGeneSelect = random();
        if (randParentGeneSelect > 0.5)
        {
            child.cmds[j] = parent1.cmds[j];
        }
        else
        {
            child.cmds[j] = parent2.cmds[j];
        }
      }
    }
    
    // Recombines the nucleotides in the organism's DNA string
    // This is done to create more diversity in the population while keeping the individual organisms relatively similar
    let cutpoint1 = random(child.cmds.length)
    // The first portion of the genetic code will remain the same
    let part1 = child.cmds.slice(0, cutpoint1)
    let cutpoint2 = cutpoint1
    // The second portion of the genetic code will be reversed
    let part2 = child.cmds.slice(cutpoint2).reverse()
    // Nucleotides closer to the start of the string will have a larger affect on the appearnce of the organisms
    // In an attempt to keep as many of the organisms as similar as possible the first portion of the DNA string will remain unchanged (not including mutations)
    child.cmds = part1.concat(part2)
    
    // The max length of an organism's DNA string is 50
    // An organism's DNA string can only ever hold 50 nucleotides
    if (child.cmds.length > 50) 
    {
      child.cmds.length =  50;
    }
    
    // Adds the new organism to the population
    newpop[i] = child
  }
  // Updates the population
  pop = newpop;
}

/**
*  Draws the organisms
**/
function draw() 
{
  // Goes through each of the organisms
  for (let i=0; i<pop.length; i++) 
  {
    // The selected orgainisms will be highlighted
    // This is simply used as a visual indicator
    if (i == parent1Index || i == parent2Index)
    {
        draw2D.color(1,0,0);    
    }
    // Default colour for organisms is white
    else
    {
        draw2D.color(1,1,1);
    }
    // Gets the current organisms row and column
    // This is calculated from the from population index `i`
    let row = Math.floor(i/cols);
    let col = i % cols;
    // Gets the local position of the organism
    draw2D.push()
      .scale(1/cols).translate(row, col)
    // Sets the current turtle
    let turtle = 
    {
      pos: new vec2(0.5, 0.9),
      dir: new vec2(0, -0.1),
      angle: Math.PI/4,
    }
    
    // Draws each line of the turtle
    // Lines are determined by the organism's DNA strand
    let lines = turtledraw(turtle, pop[i].cmds)
    draw2D.lines(lines)
    draw2D.pop()
    // Writes the DNA strand of the organism
    write(pop[i].cmds.join(""))
  }
}

/**
*  Gets mouse input
**/
function mouse(kind, pt) 
{
  // When the left mouse button is pressed down
  if (kind == "down") 
  {
    // Gets the x/y coordinate of the mouse
    let col = Math.floor(pt[0] * cols);
    let row = Math.floor(pt[1] * cols);
    // Calculate which column/row is being selected
    let index = row + col*cols;
    
    // The observer selects the parents for the next generation by clicking on the desired organisms
    // A maximum of 2 parents must be selected to proceed to the next generation
    // A minimum of 2 parents must be selected to proceed to the next generation
    // The two parents must be different/seperate/individual organisms
    if (!parent1Selected && index != parent2Index)
    {
        parent1Index = index;
        parent1Selected = true;
    }
    else if (parent1Selected && index == parent1Index)
    {
        parent1Index = -1;
        parent1Selected = false;
    }
    else if (parent2Selected && index == parent2Index)
    {
        parent2Index = -1;
        parent2Selected = false;
    }
    else if (!parent2Selected && index != parent1Index)
    {
        parent2Index = index;
        parent2Selected = true;
    }
  }
}

/**
* This function is called during key events
* @param  String kind The kind of event ("down", "up")
* @param  String key  The key (or keycode) that has been pressed/released
**/
function key(kind, key) 
{
  // The observer must press the space key to proceed to the next generation
  if (key == " ")
  {
      if (kind == "down")
      {
          // Checks to make sure 2 parents are selected
          if (parent1Index >= 0 && parent2Index >= 0)
          {
              regenerate(parent1Index, parent2Index)
              parent1Selected = false;
              parent2Selected = false;
              parent1Index = -1;
              parent2Index = -1;  
          }
      }    
  } 
}

/**
*  Description:
*        Select up to two organisms to be the parents for the next generation
*        The appearance of the organisms in the next generation will be dependent on the parents selected
*            Selecting two parent with similar appearances will streamline the population's appearance as a whole
*            Selecting two parents with different appearances will create a more diverse population
*        The population will change/evolve based on the selected parents
*  Technical Realization:
*        I was inspired by the phylogenetic tree breeding.
*        The initial population will start off with several different and unique organisms
*        As the observer continues to select two parents, certain attributes of the generations will continue to become more and more apparent
*        There is genetic mutation and recombination to ensure that the population does not become stagnant
*        If the observer choses to restart, they must restart completely "from scratch"
*  Future Extension:
*        For future extensions I could add nucleotides that affect the organisms color. 
*        However, this would require me to change the "highlight"/"selection" of the parents. 
*        The current system of selecting the parents will highlight them in red. 
*        This will not work for organisms with color as there may be red organisms
*        Alternatively I could make the organisms into agents, letting them move around the field and interact with one another
*        Or I could change the organisms and field into 3D
**/
