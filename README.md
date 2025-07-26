# combining-two-elements
def combine_elements(list1, list2):
    """
    Combines two lists of elements with position ranges, merging when more than half overlaps.
    
    Args:
        list1: First list of elements with positions and values
        list2: Second list of elements with positions and values
    
    Returns:
        Combined and merged list of elements sorted by left position
    """
    # Combine both lists
    combined = list1 + list2
    
    # Sort by left position
    combined.sort(key=lambda x: x['positions'][0])
    
    i = 0
    while i < len(combined) - 1:
        current = combined[i]
        next_elem = combined[i+1]
        
        # Calculate overlap and element lengths
        current_left, current_right = current['positions']
        next_left, next_right = next_elem['positions']
        
        current_len = current_right - current_left
        next_len = next_right - next_left
        
        overlap_left = max(current_left, next_left)
        overlap_right = min(current_right, next_right)
        overlap_len = max(0, overlap_right - overlap_left)
        
        # Check if more than half of one element is contained in the other
        if overlap_len > 0.5 * min(current_len, next_len):
            # Merge the two elements
            new_left = min(current_left, next_left)
            new_right = max(current_right, next_right)
            new_values = current['values'] + next_elem['values']
            
            # Create merged element
            merged = {
                'positions': [new_left, new_right],
                'values': new_values
            }
            
            # Replace current and remove next
            combined[i] = merged
            combined.pop(i+1)
        else:
            i += 1
    
    return combined

# Example usage
if __name__ == "__main__":
    list1 = [
        {'positions': [1, 5], 'values': ['A', 'B']},
        {'positions': [8, 12], 'values': ['C']}
    ]
    
    list2 = [
        {'positions': [3, 7], 'values': ['D']},
        {'positions': [9, 11], 'values': ['E', 'F']}
    ]
    
    combined = combine_elements(list1, list2)
    print("Combined and merged elements:")
    for elem in combined:
        print(elem)
