what is an object? root class in ruby, an instance of a class, and ruby classes themselves descend from the Object root class

A ||= B => A = A || B => lazy assignment; if A is falsy, then A = B; otherwise A = A and is not reassigned

unit testing: simply put, testing methods and the smallest unit in OOP.
primary way to achieve this is to assert that the expected result equals the actual result.

## Prompt: Design a parking lot using object-oriented principles. Don't spend too much time fleshing out actual methods; aim to give holistic view of which methods exist for each of the classes.

class ParkingLot
  attr_reader :num_large_spaces, :num_of_small_spaces, :prices

  def initialize(num_large_spaces, num_of_small_spaces, prices)
    @spaces = Set.new

    num_large_spaces.times do |id| 
      space = ParkingSpace.new("large", id)
      @spaces.add(space) 
    end

    num_small_spaces.times do |id| 
      space = ParkingSpace.new("large", id + num_large_spaces)
      @spaces.add(space) 
    end

    @prices = prices # array of hashes, one per vehicle size; keys will be amounts of time, values will the prices to park that vehicle for that much time
  end

  def num_available_spaces(vehicle_size)
    # look at the total number spaces - number of filled spaces
    # possibly more specific: if we have different kinds of spaces for different kinds of vehicles
  end

  def intake_vehicle
    # gets info about vehicle size, identifying information (make/model, license plate)
    # check if there is space for the vehicle
    # assign space
  end

  def assign_space
    # record the time at which the vehicle at which vehicle arrives
    # assign a parking space
    ### space.occupy! => record fact that space is occupied
  end

  def release_vehicle
    # record the time of departure
    # calculate the price owed
    # vacate the parking space that had been occupied, reset its @time_occupied to nil
    # collect payment, raise a gate to let the car leave
  end
end

class ParkingSpace
  attr_reader :size

  def initialize(size, id_number)
    @size = size
    @occupied = false
    @id_number = id_number
    @time_occupied = nil
  end

  def occupied?
    @occupied
  end

  def occupy_space!
    raise "Space already occupied" if occupied?
    @occupied = true
  end

  def vacate_space!
    raise "Space already vacated" unless occupied?
    @occupied = false
  end
end

<!-- class TicketStub
  attr_reader :car_size, :time_in

  def initialize(car_size, time_in)
    @car_size, @time_in = car_size, time_in
  end
end -->


implement dfs. assume Node class with value. monkey patch. starting at a root node. takes target and proc as arguments.

class Node                                                                      # A: B, C; B: D, E; C: F, G

  def dfs(target = nil, &prc)                                                   # A.dfs(:C)
    raise "must pass either a target or a proc" if target.nil? && prc.nil?

    prc ||= Proc.new { |node| node.value == target }

    return self if prc.call(self)
    self.children.each do |child_node|                                          # B, C; D, E
      search_result = child_node.dfs(&prc)                                      # A.dfs call pauses on line 99: B.dfs(&prc); B.dfs pauses => D.dfs
      return search_result unless search_result.nil?
    end

    nil
  end

end