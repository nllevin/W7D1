classes are a blueprint for constructing computer models for real or virtual objects

classes hold data ie. instance variables, methods that interact with that data, and are used
instantiate objects

self always refers to the current object
in ruby classes are always objects
inside of an instance method self is the instance of the class
at the level of an instance method, self will refer to the instance that received
that method

Design a musical jukebox using object oriented principles
Jukebox

person gives jukebox money so they can select a song to be added to the song queue
play the songs in a FIFO

Display:
show the song queue
show current artist/band and the song name
show the volume

Settings:
adjust volume

class Jukebox
    def initialize(music_library)
        raise "music_library isn't a array" unless music_library.is_a?(Array)
        @music_library = music_library
        @song_queue = []
        @money_in = 0
    end

    def get_music_library
        self.music_library
    end

    def add_to_music_library(song)
        self.music_library << song
    end

    def remove_from_music_library(song)
        self.music_library.delete(song)
    end

    def add_song_to_queue(song)
        self.song_queue << song
    end

    private
    attr_accessor :music_library
    attr_accessor :song_queue
end

implement bfs
assume there's already a node class
node class will take in a value as part of its initialization *has a value
write a bfs method starting at a root node, 2 args (target, &prc)
class Node
    def bfs(target = nil, &prc)
        prc ||= Proc.new {|node| node.value == target}
        
        queue = []
        queue << self
        until queue.empty?
            current_node = queue.shift
            return current_node if prc.call(current_node)
            current_node.children do |child_node|
                queue << child_node
            end
        end
        nil
    end
end